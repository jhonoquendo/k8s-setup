

## CAMBIAR CONTEXT
```bash
kubectl config get-contexts
kubectl config use-context <nombre_del_contexto>
```
## COMANDOS PARA CREAR PVC 
```bash
kubectl create namespace sonar-space
kubectl apply -f jenkins-pvc.yaml -n sonar-space
kubectl apply -f postgres-pvc.yaml -n sonar-space
kubectl apply -f sonarqube-pvc.yaml -n sonar-space
```
## APLICANDO LOS DEPLOYMENT Y SERVICES

```bash
kubectl apply -f sonarqube.yaml -n sonar-space
kubectl apply -f jenkins.yaml -n sonar-space
kubectl apply -f postgres.yaml -n sonar-space
```
## VALIDAR LOS RECURSOS DENTRO DE NAMESPACE

```bash
kubectl get all -n sonar-space
```

## VALIDAR PODS

```bash
kubectl get pods -n sonar-space
```

## VALIDAR PVCs
```bash
kubectl get pvc -n sonar-space
```
## PARA ELIMINAR DEPLOYMNETS Y PVC:
```bash
kubectl delete deploy sonarqube -n sonar-space
kubectl delete pvc sonarqube-pvc -n sonar-space

kubectl delete deploy jenkins -n sonar-space
kubectl delete pvc jenkins-pvc -n sonar-space

kubectl delete deploy postgres -n sonar-space
kubectl delete pvc postgres-pvc -n sonar-space
```
## ELIMINAR TODO DE UN NAMESPACE
```bash
kubectl delete all --all -n sonar-space
```
## ELIMINAR PVC DE UN NAMESPACE
```bash
kubectl delete pvc --all -n sonar-space
```
## EXPONER PUERTOS - PORT FORWARD
```bash
kubectl port-forward service/postgres -n sonar-space 5432:5432
kubectl port-forward sonarqube-0  -n sonar-space 9000:9000
kubectl port-forward jenkins-0  -n sonar-space 8080:8080
```
## VER LOG DE JENKINS Y SONAR
```bash
kubectl logs jenkins-86b667df5d-dpcw6 -n sonar-space
kubectl logs sonarqube-68d96fd5f9-xrp6s -n sonar-space
```

## VER RECURSOS DENTRO DE UN NAMESPACE
```bash
kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get -n sonar-space
kubectl api-resources --verbs=list --namespaced -o name | while read resource; do echo "$resource:"; kubectl get -n sonar-space $resource --no-headers | wc -l; done

kubectl api-resources --verbs=list --namespaced -o name `
  | ForEach-Object { 
      Write-Host "`n*** $_ ***" 
      kubectl get $_ -n sonar-space 
    }

```
## REALIZAR BACKUP DE LA BASE DE DATOS POSTGRESS: 
```bash
kubectl exec -it -n sonar-space deployment/postgres -- bash
pg_dump -U sonarqube -d sonardb -f /tmp/sonardb_backup.sql
```
## PARA SACAR EL ARCHIVO FUERA DEL POD
```bash
kubectl cp sonar-space/postgres-7c8c84cdb8-fvp5j:tmp/sonardb_backup.sql ./sonardb_backup.sql
```