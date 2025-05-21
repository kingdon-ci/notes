* Story
* Joke
* Anecdote
* Talk content
* Image Update Automation
* Flux 2.6
* Roadmap
* What is Flux
* What is Flux experience like, in detail (in 2 minutes)
	* You Git Push and your changes go out
	* Well there's a little more to it than that...
	* Components of Flux: Kustomize Controller
		* Kustomization is what?
	* Helm Controller
		* Everybody loves Helm
	* Source Controller
		* You didn't think we broke SRP, did you?
	* Notification Controller
		* The fourth essential Flux controller, today's focus
			* Responsible for:
			* Outbound Notifications (Alerts)
			* Inbound Notifications (Receivers - Webhooks)
		* Side note: but what about internal communication?
	* Are we glossing over some things?
		* https://fluxcd.io/flux/flux-e2e/#diagram-cluster-sync-from-git
		* Yeah, you betcha
	* Extra controllers:
		* Image Automation Controller & Image Reflector Controller
		* IAC: clone a Git repository, scan for markers, and update tags
		* IRC: mirror the tag list from an OCI repo (docker) in the cluster
		* ImagePolicy:
			* monitor the IRC for changes
			* and promote them to the IAC
		* ImagePolicy in 2.6:
			* it's getting a reconciler now
			* so we can finally use `:latest`
			* (That's right, digest mode is in Flux IAC ✅)
	* So that's what's in the box
		* How does it feel like using it?
* POV: I'm developing my app, I've got my platform team
	* I'm pushing a new commit to my app, it's automatically sent to the dev env
	* It LGTM, I'm merging the commit to main
	* I tagged 0.1.1 on my amazing new app
	* The platform team worked with me to come up with this ImagePolicy:

```yaml
---
apiVersion: image.toolkit.fluxcd.io/v1beta2
kind: ImagePolicy
metadata:
  name: provider-helm
  namespace: flux-system
spec:
  imageRepositoryRef:
    name: provider-helm
  policy:
    semver:
      range: 0.x
---
apiVersion: image.toolkit.fluxcd.io/v1beta2
kind: ImageRepository
metadata:
  name: provider-helm
  namespace: flux-system
spec:
  image: xpkg.upbound.io/crossplane-contrib/provider-helm
  interval: 30m
```

(Let's pretend we're building crossplane-helm, this is our app, and the latest version is going to be deployed every 30 minutes when ImageRepository syncs)

YOLO

Is there any way we can make this better?

We can make the image tag range more specific: `0.1.x` - less YOLO

There isn't any way to make this better. We aren't really staffing crossplane-contrib, so we're not in a position to set up a webhook. Open source can't really take advantage of what I'm about to tell you. It's only for enterprises and individuals.

## `Receiver` API is for Webhooks

