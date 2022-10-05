# urls | URL roting

---

*Django gets url request and uses [urls.py](http://urls.py) to select a view*

```python
from django.urls import path, include
from . import views 

urlpatterns = [
    path('', views.response),
    path('app', include('app.urls')),
]
```
