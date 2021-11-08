```python
import json
import logging
import traceback

from django.shortcuts import render
from django.http import HttpResponse
from django.views.generic import DetailView
from django.core.exceptions import PermissionDenied
from django.contrib.auth.mixins import PermissionRequiredMixin

from .models import Geocollection

logger = logging.getLogger(__name__)


class GeocollectionDetail(PermissionRequiredMixin, DetailView):
    model = Geocollection

    def has_permission(self):
        return self.request.user.has_perm('geocollection.access_geocollection', self)

def geocollection_permissions(request, collection_name):
    geocollection = Geocollection.objects.get(slug=collection_name)
    user = request.user

    if not user.has_perm('access_geocollection', geocollection):
        raise PermissionDenied

    if request.method == 'GET':
        return render(request, 'geocollections/geocollection_permissions.html', context={'object': geocollection})

    elif request.method == 'POST':
        success = True
        message = "Permissions successfully updated!"
        try:
            perm_spec = json.loads(request.POST.get('perm_spec'))
            logger.info(f" ---- setting perm_spec: {perm_spec}")
            geocollection.set_permissions(perm_spec)

            return HttpResponse(
                json.dumps({'success': success, 'message': message}),
                status=200,
                content_type='text/plain'
            )
        except Exception as e:
            traceback.print_exc()
            logger.exception(e)
            success = False
            message = f"Error updating permissions :(... error: {e}"
            return HttpResponse(
                json.dumps({'success': success, 'message': message}),
                status=500,
                content_type='text/plain'
            )
```
