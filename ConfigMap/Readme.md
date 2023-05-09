# ConfigMap, And Secret

![config](https://github.com/Jmcglobal/Kubernetes-MyProject/assets/101070055/a20650b4-95da-4081-b001-51aa2abdd489)

### What is ConfigMap.
ConfigMaps are used to separate application code from the environment-specific configuration. With ConfigMaps, there's no need to hardcode the configuration data in the Pod specification yaml configutaion file. The connection environment varibale is kept seperate and can be reference on.

![cm](https://github.com/Jmcglobal/Kubernetes-MyProject/assets/101070055/23928700-b627-4c2c-865f-0d0f9c3a1920)

![ref-cm](https://github.com/Jmcglobal/Kubernetes-MyProject/assets/101070055/9dc07aa5-75a3-4245-87fb-08bc25e86403)

#### Common Problems with env
In configMap, there is one common problem using env format to refernce the value in configMap, if the value on configMap is changed for any reason. It will result to connection failure to DB, and the common way which it can be resolved is either delete the deployment and re-apply it again. This method will result to downtime in production environmnet.

Therefore this problem issue can be corrected with volumeMounts, refferencing the ConfigMap value as file inside the pod, so that any change in configMap will automatically be updated on the pod

![cm-vom-mnt](https://github.com/Jmcglobal/Kubernetes-MyProject/assets/101070055/90b5318b-64dd-4671-9e85-97739209437d)

#### Example.....
Backend database connection url stringe, port can be stored seperately using configMap.

### What is Secret
Kubernetes Secrets are objects that store sensitive information, such as passwords, OAuth tokens, and SSH keys. They are stored in a secure and encrypted base64 encoded format, and can be used to provide secure access to applications and services.
