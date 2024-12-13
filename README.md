
# Django Project Starter Guide

This guide will help you start a Django project with models, serializers, and views. It also includes a checklist to go through before running the server.

---

## 1. Install Django

First, ensure Django is installed by running the following command:

```bash
pip install django
```

## 2. Create a Django Project
To create a new project, run:

```bash
django-admin startproject myproject
cd myproject
```

This creates the base structure for your Django project.

## 3. Create a Django App
Within the project, create a new app:

```bash

python manage.py startapp myapp
```

Now, add 'myapp' to INSTALLED_APPS in myproject/settings.py:
```bash
INSTALLED_APPS = [
    ...,
    'myapp',
]
```

## 4. Define a Model

documentation: https://docs.djangoproject.com/en/5.1/topics/db/models/

In myapp/models.py, create a simple Product model:

```bash
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100)
    description = models.TextField()
    price = models.DecimalField(max_digits=10, decimal_places=2)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.name
```

to add index to improve query performance:

```bash
class Meta:
        indexes = [models.Index(fields=['fieldName', 'fieldName'])]
```

To add conditional types

```bash
EG_TYPES = [
        ("IELTS", "IELTS"),
        ("TOEFL", "TOEFL")
    ]
    type = models.CharField(max_length=5, choices=TEST_TYPES)
```

For foregin-key, many-to-many relationship, refer to: https://docs.djangoproject.com/en/5.1/topics/db/examples/ 

To apply the model, run the following commands:

```bash
python manage.py makemigrations
python manage.py migrate
```

## 5. Create a Serializer

docs: https://www.django-rest-framework.org/api-guide/serializers/ 

In myapp/serializers.py (create this file), define a serializer for the Product model:

```bash
from rest_framework import serializers
from .models import Product

class <ModelName>Serializer(serializers.ModelSerializer):
    class Meta:
        model = <ModelName>
        fields = ['id', 'name', 'description', 'price', 'created_at']
```

to add validators inside a class

```bash
 def validate(self, data):
        # check for condition using data['fieldName']
        if fail:
            raise serializers.ValidationError('error msg')
        
        return data
```


## 6. Define Views

docs: https://www.django-rest-framework.org/tutorial/3-class-based-views/

In myapp/views.py, create views to handle API requests:

```bash
from rest_framework import generics
from .models import Product
from rest_framework.views import APIView, status

class <NameView>CreateView(APIView):
    """
    add comments
    """

    def get/post(self, request, pk):
        """
        add comments
        """
        queryset = Product.objects.all()
        filter = <ModelName>.objects.filter(id=pk).first().<fieldName>
        
        try:
            // if get request no serialzer is required
            serializer = <NameSerializer>(data={
                            'fieldName': request.data.get('fieldName'),
                            'fieldName': request.data.get('fieldName'),
                            ...
                        })
                        
                        if serializer.is_valid():
                            serializer.save()
                            return Response({'message': 'Test score submitted successfully'}, 
                                        status=status.HTTP_201_CREATED)
                        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
            
        except Exception as e:
            # Log the error here
            return Response(
                {'error': 'Something went wrong while processing your request'},
                status=status.HTTP_500_INTERNAL_SERVER_ERROR
           )

```

to add logs

```bash
import logging
logger = logging.getLogger(__name__)

class PostCredentials(APIView):
    def post(self, request, format=None):
        try:
            logger.info(f"Processing test score submission for student {request.data.get('student')}")
```

## 7. Set Up URLs

docs: https://docs.djangoproject.com/en/5.1/topics/http/urls/

In myapp/urls.py, wire up the views:

```bash
from django.urls import path
from .views import ProductListCreateView, ProductDetailView

urlpatterns = [
    path('products/', ProductListCreateView.as_view(), name='product-list-create'),
    path('products/<int:pk>/', ProductDetailView.as_view(), name='product-detail'),
]

```
Include the app URLs in myproject/urls.py:

```bash
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('myapp.urls')),
]
```

## 9. Checklist Before Running the Server
Migrations: Ensure all migrations are applied:

```bash
python manage.py migrate
```


```bash
Settings: Check INSTALLED_APPS for 'myapp' and 'rest_framework'.

URLs: Ensure URLs are correctly set in both myapp/urls.py and myproject/urls.py.
```

## 10. Run the Server
Start the Django development server:

```bash
python manage.py runserver
Your API will be accessible at http://127.0.0.1:8000/api/products/.
```


