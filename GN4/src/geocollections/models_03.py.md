```python
import logging
import traceback

from django.db import models
from django.conf import settings
from django.contrib.auth.models import Group, Permission
from django.contrib.auth import get_user_model
from django.contrib.contenttypes.models import ContentType
from django.core.exceptions import ObjectDoesNotExist

from guardian.utils import get_user_obj_perms_model
from guardian.shortcuts import assign_perm, get_groups_with_perms
from guardian.models import UserObjectPermission, GroupObjectPermission

from geonode.base.models import ResourceBase
from geonode.groups.models import GroupProfile

logger = logging.getLogger(__name__)


class Geocollection(models.Model):
    """
    A collection is a set of resources linked to a GeoNode group
    """
    group = models.ForeignKey(GroupProfile, related_name='group_collections', on_delete=models.CASCADE)
    resources = models.ManyToManyField(ResourceBase, related_name='resource_collections')
    name = models.CharField(max_length=128, unique=True)
    slug = models.SlugField(max_length=128, unique=True)

    def get_users_with_perms(self):
        """
        Override of the Guardian get_users_with_perms
        """
        ctype = ContentType.objects.get_for_model(self)
        permissions = {}

        for perm in Permission.objects.filter(codename='access_geocollection', content_type_id=ctype.id):
            permissions[perm.id] = perm.codename

        user_model = get_user_obj_perms_model(self)
        users_with_perms = user_model.objects \
            .filter(
                object_pk=self.pk,
                content_type_id=ctype.id,
                permission_id__in=permissions) \
            .values('user_id', 'permission_id')

        users = {}
        for item in users_with_perms:
            if item['user_id'] in users:
                users[item['user_id']].append(permissions[item['permission_id']])
            else:
                users[item['user_id']] = [permissions[item['permission_id']], ]

        profiles = {}
        for profile in get_user_model().objects.filter(id__in=list(users.keys())):
            profiles[profile] = users[profile.id]

        return profiles

    def remove_object_permissions(self):
        UserObjectPermission.objects \
            .filter(content_type=ContentType.objects.get_for_model(self),
                    object_pk=self.id) \
            .delete()
        GroupObjectPermission.objects \
            .filter(content_type=ContentType.objects.get_for_model(self),
                    object_pk=self.id) \
            .delete()

    def set_default_permissions(self):
        """
        Set default permissions.
        """
        # remove all permissions
        self.remove_object_permissions()

        # default permissions for anonymous users
        anonymous_group, created = Group.objects.get_or_create(name='anonymous')

        # assign permissions to 'Anyone' if 'DEFAULT_ANONYMOUS_VIEW_PERMISSION' is True
        if settings.DEFAULT_ANONYMOUS_VIEW_PERMISSION:
            assign_perm('access_geocollection', anonymous_group, self)

        # default permissions to the Geocollection group members
        assign_perm('access_geocollection', self.group.group, self)

    def set_permissions(self, perm_spec):
        anonymous_group = Group.objects.get(name='anonymous')
        self.remove_object_permissions()

        if 'users' in perm_spec and "AnonymousUser" in perm_spec['users']:
            if perm_spec['users']['AnonymousUser']:
                for perm in perm_spec['users']['AnonymousUser']:
                    assign_perm(perm, anonymous_group, self)

        if 'users' in perm_spec:
            for username, perms in perm_spec['users'].items():
                if get_user_model().objects.filter(username=username).exists():
                    user = get_user_model().objects.get(username=username)
                    logger.error(f" assign_perm '{user}' -> {perms}")
                    for perm in perms:
                        assign_perm(perm, user, self)

        if 'groups' in perm_spec:
            for group, perms in perm_spec['groups'].items():
                group = Group.objects.get(name=group)
                for perm in perms:
                    assign_perm(perm, group, self)

    def get_all_level_info(self):
        resource = self.get_self_resource()
        users = self.get_users_with_perms()
        groups = get_groups_with_perms(
            resource,
            attach_perms=True)
        if groups:
            for group in groups:
                try:
                    group_profile = GroupProfile.objects.get(slug=group.name)
                    managers = group_profile.get_managers()
                    if managers:
                        for manager in managers:
                            if manager not in users and not manager.is_superuser and manager != resource.owner:
                                assign_perm('access_geocollection', manager, resource)
                                users[manager] = ['access_geocollection', ]
                except GroupProfile.DoesNotExist:
                    tb = traceback.format_exc()
                    logger.debug(tb)
        if resource.group:
            try:
                group_profile = resource.group
                managers = group_profile.get_managers()
                if managers:
                    for manager in managers:
                        if manager not in users and not manager.is_superuser and manager != resource.owner:
                            assign_perm('access_geocollection', manager, resource)
                            users[manager] = ['access_geocollection', ]
            except GroupProfile.DoesNotExist:
                tb = traceback.format_exc()
                logger.debug(tb)

        info = {
            'users': users,
            'groups': groups
        }

        return info

    def get_self_resource(self):
        try:
            if hasattr(self, "resourcebase_ptr_id"):
                return self.resourcebase_ptr
        except ObjectDoesNotExist:
            pass
        return self

    def __str__(self):
        return self.name

    class Meta:
        permissions = (
            ('access_geocollection', 'Can view geocollection'),
        )
```
