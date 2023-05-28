# <h1 align="center"> ❗❌Proyecto: Tolerante a Fallas❌❗
	
👉Aplicación web desplegada en Flask para el consumo de distintas APIS
	
![image](https://github.com/HugoT981/ToleranteAFallas/assets/89029549/54834769-d950-4a39-ae14-7bdffacc0d62)
	
# 🔗APIS en uso:
 - **✨GIPHY:** API que nos ayuda a generar búsquedas de gifs de cualquier tema.
 - **🕷MARAVILLA:** API que nos muestra varios personajes de marvel, dando clic en alguno, se direccionará a la página de marvel en donde se encontrarán los comics donde aparece dicho personaje.
 - **🐶PERRO:** API que nos muestra una imagen random de alguna raza de perritos, pero en la barra de búsqueda podremos buscar alguna raza en específico.
 - **💣PIELES DE BACALAO:** API en fase de pruebas.

# 👥Autores:
 👦 Hernandez Ángel	
	
 👱‍♀️ Ochoa Leos Melanie Alejandra
	
 🧑 Suárez Torres Hugo Enrique
	
# 🌐Tecnologías en uso:
 - HTML
 - CSS
 - JS
 - PYTHON (flask)
 - DOCKER
 - KUBERNETES
 - ISTIO
	
# 📌Comenzando:
*Las siguientes instrucciones te permitirán obtener una copia del proyecto en funcionamiento en un contenedor de docker, con el cual podrás hacer uso de nuestras APIS.*
	
## 🔎Pre-requisitos:
- Docker
- Python
- Flask
- Minikube
- Istio
	
## 🐳Docker
Descargamos la imagen de docker con el siguiente comando:
> docker pull HugoT981/proyecto
	
Se puede verificar que nuestra imagen se creó correctamente con:
> docker images
	
## 🕸Kubernetes
Iniciamos minikube:
> minikube start
	
Cargamos los deployment y service para generar los pods:
> kubectl apply -f proyecto-v1.yaml
	
![image](https://github.com/HugoT981/ToleranteAFallas/assets/89029549/afa2d0a8-7d65-4bde-980e-8bfd68ff3ae9)

Observamos que hayan sido generados correctamente:
> kubectl get pods
> minikube dashboard
	
![image](https://github.com/HugoT981/ToleranteAFallas/assets/89029549/5e0849e0-8c0b-46bf-a0db-d162b41c9feb)
	
## 📂Istio
Aplicamos una inyección a nuestros pods:
> ubectl label namespace default istio-injection=enabled
	
Aplicamos los cambios a los archivos para que se les inyecte el proxy a los pods:
> kubectl apply -f proyecto-v1.yaml
	
Ahora verificamos que la inyeccion se realizó correctamente:
	
![image](https://github.com/HugoT981/ToleranteAFallas/assets/89029549/9c8b66bd-5d87-4571-9639-aaf08b5f07fa)

> kubectl get pods
> kubectl describe pods <pod_name>

💡Al hacer el describe a un pod, se podrá apreciar que se tienen ahora dos contenedores, uno para la app y el otro como proxy. De esta forma, nos podremos asegurar de que en caso de una caída, caiga primero el proxy y después nuestra app.

Para iniciar el modo iteractivo de nuestro pod:
> kubectl exec -it <pod_name> /bin/sh

Para generar peticiones para la obtención de tráfico:
> while sleep 1; do curl -o /dev/null -s -w %{http_code} http://<name_service>-svc:80/; done
	
Para ver las métricas, el tráfico y estadísticas de los pods y servicios, ejecutar los siguientes comandos:
- Kiali:
  > istioctl dashboard kiali

- Prometheus:
  > istioctl dashboard prometheus

- Grafana:
  > istioctl dashboard grafana
	
- Jaeger:
  > istioctl dashboard jaeger
	
# 📌Por último:
El propósito de este proyecto es mostrar alguna forma de tolerancia a fallas, en nuestro caso, dentro de nuestra app.
	
Esto nosotros lo aplicamos y llevamos a cabo al momento de presionar la última de las APIs, la llamada "Pieles de Bacalao", y lo que hace es que nos direcciona a una página de error en donde permanecemos por 4 segundos y después somos direccionados autómaticamente a nuestra página de inicio.
	
![image](https://github.com/HugoT981/ToleranteAFallas/assets/89029549/b38435ce-b8fd-4559-9d73-d5b67ec7ba28)

Y esto gracias a las líneas de código encargadas del error 404:
	
![image](https://github.com/HugoT981/ToleranteAFallas/assets/89029549/4053e2e3-f438-4e40-8fbd-b745ea3eb8ff)



