# Network Policies

Playing around with securing network policies whilst using load balancers
for semi-trusted internal access.

Needs further testing.

```
stew ------------.
External          \
Network            \
                    \
- - - - - - - - - - -\- - - - - - - - - - -
                      \
Internal               \
Network  a ---.         \
         |     `-- c ---  c_LB
         b


Network Policy:

- c allows a
- c allows external_ip(c_LB)  # Applying a network policy will stop the LoadBalancer acting as an ingress
                              # for the service, so we explicitly allow the external IP of the LoadBalancer.
                              # Alternatively, we could allow ranges and add Except clauses for the Kubernetes
                              # Internal network

Load Balancer:

- c_LB allows public_ip(stew)


Results:

a can call b
a can call c
a can call c via external LB IP
b can call a
b cannot call c
b cannot call c via external LB IP
stew can call c via external LB IP
```
