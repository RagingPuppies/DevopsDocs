# Flux issues

## Immutable objects, Helmrelease fails

Kubernetes helm release throws exceptions on reconciliation:

    failed to replace object: Service \"whatever-service\" is invalid: spec.clusterIP: Invalid value: \"\": field is immutable" action=upgrade 
So it seems that the 'upgrade.force' strategy trying to apply changes forcefuly (helm) and when removed from git changes were not applied.
I've ran `kubectl edit hr` and removed the upgrade fields.
once done i've ran `flux reconcile helmrealse <whatever>` and issues was solved.
