## **Channels (WebSockets)**

Django Channels extends Django to handle asynchronous protocols like WebSockets, HTTP2, and more—beyond the standard HTTP request/response cycle.

---

### **1. Installation**

Install `channels`:

```bash
pip install channels
```

Update `INSTALLED_APPS` in `settings.py`:

```python
INSTALLED_APPS = [
    ...,
    'channels',
]
```

---

### **2. ASGI Configuration**

Set ASGI application in `settings.py`:

```python
ASGI_APPLICATION = 'myproject.asgi.application'
```

Create or edit `asgi.py`:

```python
import os
from channels.routing import ProtocolTypeRouter, URLRouter
from django.core.asgi import get_asgi_application
from channels.auth import AuthMiddlewareStack
import app.routing

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'myproject.settings')

application = ProtocolTypeRouter({
    'http': get_asgi_application(),
    'websocket': AuthMiddlewareStack(
        URLRouter(
            app.routing.websocket_urlpatterns
        )
    ),
})
```

---

### **3. WebSocket Routing (`app/routing.py`)**

```python
from django.urls import re_path
from . import consumers

websocket_urlpatterns = [
    re_path(r'ws/chat/(?P<room_name>\w+)/$', consumers.ChatConsumer.as_asgi()),
]
```

---

### **4. WebSocket Consumer (`app/consumers.py`)**

```python
import json
from channels.generic.websocket import AsyncWebsocketConsumer

class ChatConsumer(AsyncWebsocketConsumer):
    async def connect(self):
        self.room_name = self.scope['url_route']['kwargs']['room_name']
        self.room_group_name = f'chat_{self.room_name}'

        await self.channel_layer.group_add(self.room_group_name, self.channel_name)
        await self.accept()

    async def disconnect(self, close_code):
        await self.channel_layer.group_discard(self.room_group_name, self.channel_name)

    async def receive(self, text_data):
        data = json.loads(text_data)
        message = data['message']

        await self.channel_layer.group_send(
            self.room_group_name,
            {
                'type': 'chat_message',
                'message': message
            }
        )

    async def chat_message(self, event):
        message = event['message']
        await self.send(text_data=json.dumps({'message': message}))
```

---

### **5. Add Channels Layer Backend**

In `settings.py`:

```python
CHANNEL_LAYERS = {
    'default': {
        'BACKEND': 'channels_redis.core.RedisChannelLayer',
        'CONFIG': {
            "hosts": [('127.0.0.1', 6379)],
        },
    },
}
```

Install Redis and `channels-redis`:

```bash
pip install channels_redis
```

---

### **6. Frontend WebSocket Example**

```html
<script>
    const roomName = "testroom";
    const chatSocket = new WebSocket(
        'ws://' + window.location.host + '/ws/chat/' + roomName + '/'
    );

    chatSocket.onmessage = function(e) {
        const data = JSON.parse(e.data);
        console.log('Received:', data.message);
    };

    chatSocket.onopen = function() {
        chatSocket.send(JSON.stringify({
            'message': 'Hello WebSocket!'
        }));
    };
</script>
```

---

### **7. Run with Daphne**

Install Daphne:

```bash
pip install daphne
```

Run server:

```bash
daphne myproject.asgi:application
```

Or using `manage.py` if configured correctly:

```bash
python manage.py runserver
```

---
