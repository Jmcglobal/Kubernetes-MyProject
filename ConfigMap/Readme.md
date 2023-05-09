# ConfigMap

### What is ConfigMap.
ConfigMaps are used to separate application code from the environment-specific configuration. With ConfigMaps, there's no need to hardcode the configuration data in the Pod specification yaml configutaion file. The connection environment varibale is kept seperate and can be reference on.

#### Example.....
Backend database connection stringe can be stored seperately using configMap.
