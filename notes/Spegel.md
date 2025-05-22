On the EKS cluster, I think we'll be able to solve the problem of larger-than-average image pulls and cold starts causing a long delay between instance creation and pod ready time with [[Seekable OCI]]. In our Talos cluster, we've been using a pull-through registry cache (which is a SPOF in our infrastructure) to help us solve this problem.

https://spegel.dev/

Spegel is a Stateless cluster-local OCI registry mirror that uses the node's own local CRI storage to cache image layers and serve them for other nodes in the cluster. It is production-ready, used by many companies to lower image pull latency and speed up recovery times; does not introduce a Single Point of Failure in the architecture.

There is an active community with weekly meetings on Google Meet.

Spegel works in such a way that image pulls will always fall back to the original registry if it can't be reached, or doesn't have the image ready to serve in its cache yet. So it does not create any significant additional risks for availability to install it.

Benchmarks show that Spegel can be over 60% faster than a local image registry. Seekable OCI might be even faster, but these strategies also might not be mutually exclusive. We had to choose Seekable OCI because we are using EKS Auto Mode.