{% method %}
## Post: App Authorization {#post-app-authorization}

A grant of permissions to a specific client for a given app. This allows for multiple clients to use the same app in parallel, and for them to be individually revoked if necessary.

The permissions on the Authorization {{ protocol-post }} can differ from the ones specified on the app. It's validity can also be limited in time.

{% common %}
Here's the schema for an Authorization {{ protocol-post }}:

{% sample lang="http" %}
qwe

{% endmethod %}
