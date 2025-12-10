# Misc

1. Command to purge minikube clusters

   ```bash
   minikube delete --all --purge
   ```

2. Docker API version issue fix in powershell or set in system environment variable

   ```powershell
   $env:DOCKER_API_VERSION=1.44
   ```

3. Tried of typing kubectl for minikube, use below alias or set in your shell profile

   ```bash
   function kubectl { minikube kubectl -- @args }
   ```
