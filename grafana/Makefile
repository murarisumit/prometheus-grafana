delete:
	helm delete --purge grafana

get-admin-password:
	@echo "Username: admin, password $(shell kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode)"

install:
	helm install --name grafana stable/grafana --version 1.19.0 --namespace monitoring -f values.yaml
	@echo "Try login with username: admin and password : $(kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode)"

logs:
	kubectl logs $(shell kubectl get pods --namespace monitoring -l "app=grafana" --no-headers -o jsonpath="{.items[0].metadata.name}") -n monitoring

ls:
	kubectl get pods --namespace monitoring -l "app=grafana"

port-forward:
	kubectl --namespace monitoring port-forward $(shell kubectl get pods --namespace monitoring -l "app=grafana" -o jsonpath="{.items[0].metadata.name}") 3000


