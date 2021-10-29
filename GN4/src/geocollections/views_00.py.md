```python
from django.views.generic import DetailView

from .models import Geocollection


class GeocollectionDetail(DetailView):
    model = Geocollection
```
