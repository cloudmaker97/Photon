<h1 align="center">
  <br>
  <a href="https://github.com/s0md3v/Photon"><img src="https://image.ibb.co/h5OZAK/photonsmall.png" alt="Photon"></a>
  <br>
  Photon
  <br>
</h1>

<h4 align="center">Incredibly fast crawler designed for OSINT.</h4>

<p align="center">
  <a href="https://github.com/s0md3v/Photon/releases">
    <img src="https://img.shields.io/github/release/s0md3v/Photon.svg">
  </a>
  <a href="https://pypi.org/project/photon/">
    <img src="https://img.shields.io/badge/pypi-@photon-red.svg?style=style=flat-square"
         alt="pypi">
  </a>
  <a href="https://github.com/s0md3v/Photon/issues?q=is%3Aissue+is%3Aclosed">
      <img src="https://img.shields.io/github/issues-closed-raw/s0md3v/Photon.svg">
  </a>
</p>

<p align="center">
  <a href="https://github.com/s0md3v/Photon/wiki">Photon Wiki</a> •
  <a href="https://github.com/s0md3v/Photon/wiki/Usage">How To Use</a> •
  <a href="https://github.com/s0md3v/Photon/wiki/Compatibility-&-Dependencies">Compatibility</a> •
  <a href="https://github.com/s0md3v/Photon/wiki/Photon-Library">Photon Library</a> •
  <a href="#contribution--license">Contribution</a> •
  <a href="https://github.com/s0md3v/Photon/projects/1">Roadmap</a>
</p>

### Key Features

#### Data Extraction
Photon can extract the following data while crawling:

- URLs (in-scope & out-of-scope)
- URLs with parameters (`example.com/gallery.php?id=2`)
- Intel (emails, social media accounts, amazon buckets etc.)
- Files (pdf, png, xml etc.)
- Secret keys (auth/API keys & hashes)
- JavaScript files & Endpoints present in them
- Strings matching custom regex pattern
- Subdomains & DNS related data

The extracted information is saved in an organized manner or can be [exported as json](https://github.com/s0md3v/Photon/wiki/Usage#export-formatted-result).

![save demo](https://image.ibb.co/dS1BqK/carbon_2.png)

#### Flexible
Control timeout, delay, add seeds, exclude URLs matching a regex pattern and other cool stuff.
The extensive range of [options](https://github.com/s0md3v/Photon/wiki/Usage) provided by Photon lets you crawl the web exactly the way you want.

#### Genius
Photon's smart thread management & refined logic gives you top notch performance.

Still, crawling can be resource intensive but Photon has some tricks up it's sleeves. You can fetch URLs archived by [archive.org](https://archive.org/) to be used as seeds by using `--wayback` option.

#### Plugins
- **[wayback](https://github.com/s0md3v/Photon/wiki/Usage#use-urls-from-archiveorg-as-seeds)**
- **[dnsdumpster](https://github.com/s0md3v/Photon/wiki/Usage#dumping-dns-data)**
- **[Exporter](https://github.com/s0md3v/Photon/wiki/Usage#export-formatted-result)**

#### Docker

Photon ships as a lightweight, multi-architecture Docker image published to the GitHub Container Registry (GHCR). The image is built for both `linux/amd64` and `linux/arm64`, so it runs natively on Linux servers, Intel Macs **and Apple Silicon (M1/M2/M3/M4)** without emulation.

Crawl results are written to `/app/data` inside the container — mount a local folder there to access them on your host.

**Supported platforms**

| Platform | Architecture | Status |
|----------|--------------|--------|
| Linux    | `amd64`      | Native |
| Linux    | `arm64`      | Native |
| macOS (Intel)         | `amd64` | Native |
| macOS (Apple Silicon) | `arm64` | Native |
| Windows (WSL2)        | `amd64` | Native |

**Available tags**

| Tag                  | Description                                |
|----------------------|--------------------------------------------|
| `master` / `main`    | Latest commit on the default branch        |
| `vX.Y.Z`             | Specific release version                   |
| `X.Y`                | Latest patch of a minor release            |
| `sha-<commit>`       | Build pinned to a specific commit          |

**Pull & run the prebuilt image**
```bash
docker run --rm -it \
  -v "$PWD/data:/app/data" \
  ghcr.io/cloudmaker97/photon:master \
  -u google.com
```

Docker automatically pulls the matching architecture for your machine — no `--platform` flag needed.

**Build the image locally**
```bash
git clone https://github.com/cloudmaker97/Photon.git
cd Photon
docker build -t photon .
docker run --rm -it -v "$PWD/data:/app/data" photon:latest -u google.com
```

**Build a multi-arch image locally (optional)**

If you want to reproduce the registry build (both architectures) on your own machine:
```bash
docker buildx create --use --name photon-builder
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t photon:multiarch \
  --load .
```

**Tip — persisting results**

The `data/` directory you mount will contain the crawl output (URLs, intel, files, JSON exports). Create it before the first run if it doesn't exist:
```bash
mkdir -p data
```
