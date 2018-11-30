delete:
	helm delete --purge prometheus

install:
	helm install --name prometheus stable/prometheus --namespace monitoring

ls:
	kubectl get pods --namespace monitoring -l "app=prometheus"

port-forward:
	kubectl --namespace monitoring port-forward $(shell kubectl get pods --namespace monitoring -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}") 9090

