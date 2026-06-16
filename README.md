# image-build-cloud-provider-vsphere

This repo builds hardened, statically-linked Go binaries from
[kubernetes/cloud-provider-vsphere](https://github.com/kubernetes/cloud-provider-vsphere) and packages them in a minimal
SLE BCI ([bci-nano](registry.suse.com/bci/bci-nano)) based image.

Binaries are compiled against [`rancher/hardened-build-base`](https://github.com/rancher/image-build-base),
which provides the latest supported Go toolchain (FIPS/BoringCrypto-enabled on amd64).

## Images produced

- `rancher/hardened-cloud-provider-vsphere` — vSphere Cloud Controller Manager (CPI)

## Building locally

```sh
make build-image-all          # build for the host architecture
make image-scan               # run Trivy against the built image(s)
```

The upstream version is controlled by the `TAG` variable in the [`Makefile`](./Makefile).
A `-buildYYYYMMDD` suffix (`BUILD_META`) is appended automatically and is required on
release tags.

## Automated updates

[Updatecli](./updatecli) keeps two things current via daily PRs:

- the upstream `cloud-provider-vsphere` version (`Makefile` `TAG`), and
- the `rancher/hardened-build-base` version (`Dockerfile` `GO_IMAGE`).

## CI

- **Build**: builds every image and runs a [Trivy](https://github.com/aquasecurity/trivy) scan
  (`CRITICAL,HIGH`) on each on every PR/push.
- **Release**: on a published GitHub release, builds multi-arch images and pushes them to
  `ghcr.io/rancher`.
