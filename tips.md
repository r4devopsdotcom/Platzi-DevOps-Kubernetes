# Establece una cultura de containers en la organización
– Escribir Dockerfiles para una aplicación
– Escribir compose files para describir servicios
– Mostrar las ventajas de correr aplicaciones en contenedores
– Configurar builds automáticos de imágenes
– Automatizar el CI/CD (staging) pipeline

# Developer Experience:
Si tienes una persona nueva, debe sentirse acompañada en este proceso de por qué usamos kubernetes y quieres mantener la armonía de ese proceso.

# Elección de un cluster de producción:
Hay alternativas como Cloud, Managed o Self-managed, también puedes usar un cluster grande o múltiples pequeños.

# Recordar el uso de namespaces:
Puedes desplegar varias versiones de tu aplicación en diferentes namespaces.

# Servicios con estados (stateful)
– Intenta evitarlos al principio
– Técnicas para exponerlos a los pods (ExternalName, ClusterIP, Ambassador)
– Storage provider, Persistent volumens, Stateful set

# Gestión del tráfico Http
– Ingress controllers (virtual host routing)

# Configuración de la aplicación
– Secretos y config maps

# Stacks deployments
– GitOps (infraestructure as code)
– Heml, Spinnaker o Brigade