# Crossplane Argocd

- Cài argoCD: 

``` bash
kubectl create namespace argocd
kubectl apply -k argocd/install
```

- Lấy ra password: 

``` bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

- Cài crossplane: 

``` bash
kubectl apply -f argocd/crossplane-bootstrap/crossplane-helm-secret.yaml
kubectl apply -n argocd -f argocd/crossplane-bootstrap/crossplane.yaml
```

- Thêm file `viettelcloud-creds.conf`: 

``` bash
viettelcloud_user_token=your_token_here
viettelcloud_cmp_url=https://console.viettelcloud.vn/api
viettelcloud_project_id=38909747-33b2-4d1c-94a8-19aa32e54cc9
```

``` bash
  kubectl create secret generic viettelcloud-creds \
  -n argocd \
  --from-file=creds=./viettelcloud-creds.conf
```

- Tạo crossplane provider Viettel: 

``` bash
  kubectl apply -n argocd -f argocd/crossplane-bootstrap/crossplane-provider-viettel.yaml
  kubectl apply -n argocd -f argocd/crossplane-bootstrap/crossplane-provider-viettel-config.yaml
  kubectl apply -n argocd -f argocd/crossplane-bootstrap.yaml 
  kubectl apply -n argocd -f upbound/provider-viettel/crds/
  
```



