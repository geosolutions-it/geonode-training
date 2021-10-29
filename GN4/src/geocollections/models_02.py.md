```python
from django.db import models
from django.conf import settings
from django.contrib.auth.models import Group
from django.contrib.auth import get_user_model
from django.contrib.contenttypes.models import ContentType

from guardian.shortcuts import assign_perm
from guardian.models import UserObjectPermission, GroupObjectPermission

from geonode.base.models import ResourceBase
from geonode.groups.models import GroupProfile


class Geocollection(models.Model):
    """
    A collection is a set of resources linked to a GeoNode group
    """
    group = models.ForeignKey(GroupProfile, related_name='group_collections', on_delete=models.CASCADE)
    resources = models.ManyToManyField(ResourceBase, related_name='resource_collections')
    name = models.CharField(max_length=128, unique=True)
    slug = models.SlugField(max_length=128, unique=True)

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

    def __str__(self):
        return self.name

    class Meta:
        permissions = (
            ('access_geocollection', 'Can view geocollection'),
        )
```
