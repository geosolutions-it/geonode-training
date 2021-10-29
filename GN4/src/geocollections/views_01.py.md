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
    geocollection = Geocollection.objects.get(name=collection_name)
    user = request.user

    if user.has_perm('access_geocollection', geocollection):
        return HttpResponse(
            (f'You have the permission to access the geocollection "{collection_name}". '
             'Please customize a template for this view'),
            content_type='text/plain')
    else:
        raise PermissionDenied
```
