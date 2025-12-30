Directory structure (expected)
	argocd/
	├── Chart.yaml
	├── values.yaml

Add Argo Helm repo (once)
	helm repo add argo https://argoproj.github.io/argo-helm
	helm repo update

Create the namespace (if not exists)
	kubectl create namespace observability || true

Build Helm dependencies
	cd argocd
	helm dependency build
	
Install Argo CD into observability namespace
	helm install argocd ./ \
	  -n observability

Verify deployment
	
	kubectl get pods -n observability
	
	
Confirm service namespace
			

	kubectl get svc -n observability | grep argocd	
	
Access Argo CD UI (port-forward)

	kubectl port-forward svc/argocd-server \
	  -n observability 8080:80
	  
	  OR
	  
	nohup kubectl port-forward svc/argocd-server \
	  -n observability 8080:80 \
	  > argocd-port-forward.log 2>&1 &
		  
	
Get admin password

	kubectl get secret argocd-initial-admin-secret \
	  -n observability \
	  -o jsonpath="{.data.password}" | base64 -d



-------------------

kubectl create secret tls nginx-tls \
  --cert=tls.crt \
  --key=tls.key \
  -n observability

==========
kubectl describe secret nginx-tls -n observability
==================


base64 -w 0 tls.crt
base64 -w 0 tls.key
