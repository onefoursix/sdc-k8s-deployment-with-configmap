### A note about Environment Variables in SDC deployment manifests
Environment variables with the prefix "SDC_CONF_", like <code>SDC_CONF_SDC_BASE_HTTP_URL</code>, allow one to dynamically set properties in the deployed SDC's <code>sdc.properties</code> file. These environment variables are mapped to SDC properties by trimming the "SDC_CONF_" prefix, lowercasing the name and replacing "_" with ".". So, for example, the value set in the environment variable <code>SDC_CONF_SDC_BASE_HTTP_URL</code> will be set in <code>sdc.properties</code> as the property <code>sdc.base.http.url</code>.

However, currently, due to the behavior of the SDC image's <code>docker-entrypoint.sh</code> script, this mechanism is not able to set mixed-case properties in <code>sdc.properties</code> like <code>production.maxBatchSize</code>. This limitation may be lifted in the near future.

If you need to set mixed-case properties then you either have to bake-in the settings in the sdc.properties file packaged in a custom SDC image (as in Example 2), or set the properties in a Volume mounted over the image's sdc.properties file as shown in Examples 4 and 5.

It's also worth noting that values for environment variables with the prefix "SDC_CONF_" are written to the sdc.properties file by the SDC container's <code>docker-entrypoint.sh</code> script which forces the SDC container to have read/write access to the sdc.properties file, which may not be the case if sdc.properties is mounted with read-only access.

Best practice for now is to mount <code>sdc.properties</code> from a Volume and to avoid using <code>SDC_CONF\__</code> environment variables.