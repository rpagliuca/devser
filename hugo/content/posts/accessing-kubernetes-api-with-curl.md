---
title: "Accessing the Kubernetes API using curl"
date: 2020-09-17T09:32:52-03:00
---

The following script can be used to access the Kubernetes API using curl.

For it to work, we assume you wish to connect to the first cluster listed in your
`~/.kube/config` file, by authenticating using the first user listed in that same file.

This command is as simple as it gets, so it does not try to verify if the user
is actually associated with the cluster.

If you have more advanced needs, it is for sure a good kickstart.

<pre>
curl $(cat ~/.kube/config | yq r - clusters[0].cluster.server)/api/v1/namespaces/default/pods?limit=10 \
--cacert <(cat ~/.kube/config | yq r - clusters[0].cluster.certificate-authority-data | base64 -d) \
--cert <(cat ~/.kube/config | yq r - users[0].user.client-certificate-data | base64 -d) \
--key <(cat ~/.kube/config | yq r - users[0].user.client-key-data | base64 -d)
</pre>

