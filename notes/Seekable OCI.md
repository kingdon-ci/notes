Notes on Seekable OCI:

This guy ChatGPT, hey, he is an AWS Expert... and he clued me into this solution for our issue with pods being slow to start due to long image pull times that block.

Simply make the image pulls non-blocking! Brilliant...

[ChatGPT Speaks](https://chatgpt.com/share/682f7a9a-6dc8-8006-b4fd-a0beecf60607) on the topic of Speeding Up ECR Pulls

[blogs/aws/...-faster-container-startup-using-seekable-oci/](https://aws.amazon.com/blogs/aws/aws-fargate-enables-faster-container-startup-using-seekable-oci/)
[blogs/containers/under-the-hood-...-seekable-oci-and-aws-fargate/](https://aws.amazon.com/blogs/containers/under-the-hood-lazy-loading-container-images-with-seekable-oci-and-aws-fargate/)

It's an AWS-only solution, in spite of being open source, only AWS implements this standard. It only works for OCI images that you host yourself, only on ECR... it requires an extra step at image push time, to create a "SOCI Index" - that narrow window is one we can fit through! (And it definitely works with EKS Auto Mode.)