# ssl-and-Tls-cerificate-to-improve-security-on-k8s-
create private certificate ssl-and-Tls-cerificate-to-improve-security-on-k8s-
https://www.learnitguide.net/2020/06/how-to-create-ssltls-certificate-for.html      use this website for refrence 
https://medium.com/avmconsulting-blog/how-to-secure-applications-on-kubernetes-ssl-tls-certificates-8f7f5751d788           refrence webs
files for an demo app 
..................
deployment.yamnl 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
spec:
  selector:
    matchLabels:
      app: my-app
  replicas: 1
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app-container
          image: nginx:latest
          ports:
            - containerPort: 80

........... service.yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80

...........
ingress.yamlapiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-app-ingress
spec:
  rules:
    - host: app.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-app-service
                port:
                  number: 80
  tls: 
    - 
      hosts: 
        - app.com
      secretName: my-app-secret
      .................
      secret.yaml



      apiVersion: v1
kind: Secret
metadata:
  name: my-app-secret
type: kubernetes.io/tls
data:
  tls.crt: "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lVSUZ5aHZqR0ZIWGpUOUVWeFpUdUpqaTBqMitzd0RRWUpLb1pJaHZjTkFRRUwKQlFBd0VqRVFNQTRHQTFVRUF3d0hZWEJ3TG1OdmJUQWVGdzB5TXpBM01UTXdOelEwTVRaYUZ3MHlOREEzTVRJdwpOelEwTVRaYU1CSXhFREFPQmdOVkJBTU1CMkZ3Y0M1amIyMHdnZ0VpTUEwR0NTcUdTSWIzRFFFQkFRVUFBNElCCkR3QXdnZ0VLQW9JQkFRREJ6N2R1QzJnMzNlTXdSclpjREU4bDhSNzJOb1B1T0Jwb014RmNpMWdMYUVEbUZXUS8KbXgwbXJpY3NZdkxZRG9vNDV3Z1U5azNtaHlKUlArOWlwUVg0cjdDUXI4QXRWb2JoMnhzQStDcURGcGVsQmNkbgp0RTNCTEhSaFRGcTVwYUVmQk9LQ3U3Y3U4ZnpnVitnV1l5V1NPRUZiVnRzeHV1Mjc3Skl3NVpqWFJJL2twaXluCjAwQ1JZVk5KZTRSRVgrY0xsNWt3clZzMlZqV2tXcURWbGhybVZvank1S2E3T0Z3b1NKa3E2U1NBeEVWMk1pOUYKUi9XVEw1SE1nUjhDSncxSTZJNFJVdWlHdW51Wjd6YjdxN095UHgvbng1UUUwL2FOUmxYT2xkck93anpFVlRkSgpLdnNPVSs0TC82OEhFWDgweHI0RFQ5M3IvV3lsYnFJU2FGR0pBZ01CQUFHalV6QlJNQjBHQTFVZERnUVdCQlFYCmU1dThHSTk1MlNCME84RWVMMS9wOVZPNWlUQWZCZ05WSFNNRUdEQVdnQlFYZTV1OEdJOTUyU0IwTzhFZUwxL3AKOVZPNWlUQVBCZ05WSFJNQkFmOEVCVEFEQVFIL01BMEdDU3FHU0liM0RRRUJDd1VBQTRJQkFRQ09IWXdaNXJMbwpBWmU3c0REUkxONkI5TW9EcTM1ejBnN3FsUCtFMFpOSnFxMWprVGVKZnZLclE0SG5MSmxHRUFmUFJ5eWxKTWRhCldzYm85MjI5Q3hMVFhwcGpzZEZKbG9UaXFVVXZNT3BXMlo5L3lNKzJEWStueG9aS2F5VlJlQWdPd1ZIaUt0N1YKT3Z1b09FbFl5VFZWUitrZXJoeVM1RnV0c3VSQmt0VzRBc0xRZkE4dE5NWUQzSXhuSmxudkRSZkR5TEkwMXNheQpOdzh2ODlsUjhaVHhVWVVDRkdRbGlXRUUrdGV3VmlENGxnelBvVTU3VDZzTDEySXZKV2x1bU9iYlhWcFphWWJZCjB2cWtIem04a0xIbkZjNTJGc2I1SGhmYWFwM0E2WlBpdnFIOWo2eTU2TnJDcWw0eWczMUUxV3h3dytNYVVncE8KYUdNUTBTWlFnN3JmCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
  tls.key: "LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFb3dJQkFBS0NBUUVBd2MrM2JndG9OOTNqTUVhMlhBeFBKZkVlOWphRDdqZ2FhRE1SWEl0WUMyaEE1aFZrClA1c2RKcTRuTEdMeTJBNktPT2NJRlBaTjVvY2lVVC92WXFVRitLK3drSy9BTFZhRzRkc2JBUGdxZ3hhWHBRWEgKWjdSTndTeDBZVXhhdWFXaEh3VGlncnUzTHZIODRGZm9GbU1sa2poQlcxYmJNYnJ0dSt5U01PV1kxMFNQNUtZcwpwOU5Ba1dGVFNYdUVSRi9uQzVlWk1LMWJObFkxcEZxZzFaWWE1bGFJOHVTbXV6aGNLRWlaS3Vra2dNUkZkakl2ClJVZjFreStSeklFZkFpY05TT2lPRVZMb2hycDdtZTgyKzZ1enNqOGY1OGVVQk5QMmpVWlZ6cFhhenNJOHhGVTMKU1NyN0RsUHVDLyt2QnhGL05NYStBMC9kNi8xc3BXNmlFbWhSaVFJREFRQUJBb0lCQUhGdE1vb0p6UVdkSzRBdQpjajF1eXNlRzFUcjlicnIxUktjazlBMDRVOS9oTk5JelJNZGc3VytjenJwUWNwVjE5UGtXWlFsM21PSEl4cEhNCm5Eb2NJR0dKMlFqa2d0RkY2WXkzSVpld3BaaXdtVEZ3TDJLSENGWjh3T1BNdnZBVmVqVmdNM3lWaGNESXlOazMKT1ZJWTFuMDd4U3hDcWVmeDRYNXhGcUhkVFZUMSs0NCt3dis4N3BvMkhPL0RHYk4wczNIUFFVemJyejErbTJjOAphRFdDdFlzSFRMcUlzMU1TcU5CZkFrdjRpamE1NjBFV3pNODY0aDZmZndodkQzZDFoSVFkNVA5czBwWDVldEFtCkx6d3RBYzRwbHE0QlUwTWczMFQ1cVI2RGxHby8yR0hXc3BFWXR5VzhDZ1V2TzcyRGk2b1IyNldZeWZuUHhnRkYKRzUxdUhBMENnWUVBNFgySWVpY0F2OFNXRisxUEMybjBLSHZxblhyRmFyY0JLZ1gvYVg4SUtqdXZWR0tnMzZnVApadEpIR25Oa0VaRlRpMitzU2tGMEh6S25vTlhBYkFsNkJNSlNsRVZXNDhpRXk3c1RwaHNjTlorclU4RTRnU2JkCnR6YzRRSVV6SDhvODFiSUt5NHBXa0doQk5rc1A4UUJIa1g0OXJHaWx0NWZiRjRmOU1DWE9yOHNDZ1lFQTNBamwKbFU1QUNTOTl6b3YweFRnUHJ3OUdIem1LVmZTQS9hdUJua0pmRUplcVQ0SnlHWHJDWFJJV1liV3R3cXFJTTNLZApzazFQR1hxZnlPcmJzUCs3SDVHSk16Vm93S2EzRDExcldoUEZzalN6akk4Ti9NQmR4NGUwL2MreXlwK3VZdTIvClVtUHlzbmFWT0JHM1ZId0pvUElVNjlTSGtMeHJBV3FvMkJMY01Yc0NnWUVBcHRQQWNGZUE1MkJqaDZwTUsrNmsKOUhyUmx0ZHBUYzI5cjhDbU9nQUJJM1hxL3V6Rmh4T0wzeXU5N1dUbjZWTnkweHU1QldzdHBaTk5qK0gxTGpsSQpyRkswMC93RkVCdWZuRGQ1anhCSnE2YkpFL3RGZWRBdWcwbjRkVXZYQ2pNUEZTOVhhMFdiUzlYR1FZd0JiRlcvCm5YWWYzUG5EZVhTQlpRUjRudkdwM0VjQ2dZQjdEeXEvbHpUdkxqVnhTQXNNSmU5M205WSs3bUowOGpzV0pFNW8KNFh2amZyOU1tb3NQdnYxbktnK0VkQ1NMSSs3cXZ5WjlLd21iR1Y2MThzd05zT2pKbmc2YXFqczh5OERFQWg1aApFWC9XeSt6REp0ck95aE5vM1hnWEg0dENFWTVwVzhoTjN3SkVWMWZiTk9WUWhkS294ZHQzamJTSCtTanJjT2lmCmQrVFljd0tCZ0ZjUmRibk9UcFNESDFYbkY2NlNvSCtsTUVqVUo5UjY5ajVXMHFEQkc4SnlKMHBRMzJZRE5kbWIKbURpWllFTk5DRExTeEIvU0hCeHVFRzY4SWlIbDJjNHMyWDFtem9VK1pxZDlIWmRTTTlCM2ZoMm05K1ptSlZOWgo1Q0pKSG1qMnR6SVpOUnlZRk1ScitXYks4dG83SXczWDRrbzJWaGJEVmdGejJXeThsTTh6Ci0tLS0tRU5EIFJTQSBQUklWQVRFIEtFWS0tLS0tCg=="

for this we first create privet key 
openssl genrsa -out ca.key 2048
now we crete private certificte 
openssl req -x509 -new -nodes -days 365 -key ca.key -out ca.crt -subj "/CN=app.com"
create secreand other yaml files applt them and search app.com after entering the ip in /etc/hosts with domanin name 
cat ca.crt | base64 -w 0   commands to create encrypted format 



