**DESENVOLVIMENTO WEB II**

# Aula 9 – Criando a primeira API REST com Django REST Framework

**Objetivo:**

- Apresentar o Django REST Framework (DRF).
- Criar a primeira API REST para o modelo `Produto`
- Explorar endpoints básicos (listar, detalhar, criar, atualizar e deletar).
- Testar a API no navegador, Postman e terminal.

---

**1. O que é o Django REST Framework**

O **Django REST Framework (DRF)** é uma biblioteca poderosa e flexível para criar APIs em Django. Ele fornece serializadores, views genéricas, roteadores, autenticação, permissões e paginação integradas. É o padrão de fato para APIs com Django.

**Instalação:**

```bash
pip install djangorestframework
pip freeze > requirements.txt
```

**Configuração em `settings.py`:**

```python
INSTALLED_APPS = [
  ...,
  'rest_framework',
  'produtos',
]
```

---

**2. Criando o Serializer**

Arquivo: `produtos/serializers.py`

```python
from rest_framework import serializers
from .models import Produto

class ProdutoSerializer(serializers.ModelSerializer):
  class Meta:
    model = Produto
    fields = '__all__'
```

---

**3. Criando as Views**

Arquivo: `produtos/views.py`

```python
from rest_framework import viewsets
from .models import Produto
from .serializers import ProdutoSerializer

class ProdutoViewSet(viewsets.ModelViewSet):
  queryset = Produto.objects.all()
  serializer_class = ProdutoSerializer
```

---

**4. Criando as rotas**

Arquivo: `produtos/urls.py`

```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import ProdutoViewSet

router = DefaultRouter()
router.register(r'produtos', ProdutoViewSet)

urlpatterns = [
  path('', include(router.urls)),
]
```

No `urls.py` principal do projeto:

```python
from django.urls import path, include

urlpatterns = [
  path('api/', include('produtos.urls')),
]
```

---

**5. Testando a API**

Rodar o servidor:

```bash
python manage.py runserver
```

Acessar no navegador:

- http://127.0.0.1:8000/api/produtos/ → Listagem e criação
- http://127.0.0.1:8000/api/produtos/1/ → Detalhe, edição e exclusão

---

**Exercícios práticos**

- Criar 3 produtos usando a API.
- Listar todos os produtos e verificar no Admin se foram salvos.
- Atualizar o preço de um produto via API.
- Deletar um produto e confirmar no Admin.

---

**Encerramento:**

Nesta aula, aprendemos a expor um modelo Django como API REST usando o DRF, com endpoints automáticos para CRUD. Na próxima aula, veremos como customizar a API com campos extras, filtros, buscas e paginação.

