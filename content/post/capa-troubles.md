+++
author = "Mike Murray"
title = "Cluster-Api AWS Provider Credentials"
description = "quick tip on updating creds for cluster-api"
date = "2022-06-05"
tags = [
    "eks",
    "capa",
    "aws",
    "cluster-api",
    "TDIL",
]
+++
# capa troubles

- updating credentials for the cluster-api aws controller requires the deployment to be restarted


so today I was working on getting the cluster-api aws provider to spin up a new cluster and needed to provide updated credentials that were not expired anymore. To do that you can run `clusterawsadm controller update-credentials` to update the credentials with the info in the `AWS_B64ENCODED_CREDENTIALS` environment variable. After running the command I was able to confirm the new credentials were in the cluster and things should have been happy and ready to go. Unfortunately the controller disagreed and continued to use the expired creds for some reason. After looking through the bugs on the capa repo it turns out that the creds are cached in the controller so when the credentials are updated the deployment also needs to be restarted with `k rollout restart -n capa-system deployment capa-controller-manager`
