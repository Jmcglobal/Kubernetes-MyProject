# ConfigMap, And Secret

![config](https://github.com/Jmcglobal/Kubernetes-MyProject/assets/101070055/a20650b4-95da-4081-b001-51aa2abdd489)

### What is ConfigMap.
ConfigMaps are used to separate application code from the environment-specific configuration. With ConfigMaps, there's no need to hardcode the configuration data in the Pod specification yaml configutaion file. The connection environment varibale is kept seperate and can be reference on.

#### Example.....
Backend database connection url stringe, port can be stored seperately using configMap.

### What is Secret
Kubernetes Secrets are objects that store sensitive information, such as passwords, OAuth tokens, and SSH keys. They are stored in a secure and encrypted base64 encoded format, and can be used to provide secure access to applications and services.
