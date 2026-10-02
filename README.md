# TerraVision Cloud Diagrams

Ask Claude for a cloud architecture diagram in plain words and get the diagram a cloud architect would draw: the official AWS, Azure or Google Cloud icons, with every resource inside its VPC, subnet, zone or resource group.

This plugin connects Claude to [TerraVision](https://github.com/patrickchugh/terravision), a free, open-source cloud architecture diagram tool that works in both directions:

- **Design to code.** Describe an architecture, get the diagram, refine it by talking, then ask Claude to write the Terraform for it.
- **Code to diagram.** Point it at a Terraform folder and get a diagram of what the code deploys.

![AWS three-tier web application diagram drawn by TerraVision](https://raw.githubusercontent.com/patrickchugh/terravision/main/images/gallery/three-tier-web.png)

More examples for all three clouds are in the [gallery](https://patrickchugh.github.io/terravision/gallery/).

## What you can ask

- "Draw an AWS three-tier app: React on CloudFront, ECS Fargate behind an ALB in two AZs, SQL Server on RDS Multi-AZ"
- "Add ElastiCache and show how a request flows through it"
- "Draw the same thing on Azure"
- "Draw the architecture of the Terraform in ./infra"
- "Write the Terraform for this architecture"

You get PNG and SVG images, an editable draw.io file and the underlying graph. Files are saved in a `diagrams` folder in your project.

## What is in this plugin

- **A skill** (`skills/terravision-cloud-diagrams`) that teaches Claude the TerraVision graph format, with worked examples for AWS, Azure and Google Cloud and a small validator script.
- **A local MCP server**, TerraVision itself, which renders the diagrams.

## Requirements

- [uv](https://docs.astral.sh/uv/), which runs TerraVision
- [Graphviz](https://graphviz.org/download/), which draws the diagram
- Terraform or OpenTofu, and Git, only if you want diagrams from Terraform code

Full setup steps for macOS, Windows and Linux are in the [installation guide](https://patrickchugh.github.io/terravision/ai-assistants/).

## What this plugin runs, downloads and sends

- **Runs:** the command `uvx --from "terravision[mcp]==0.52.0" terravision mcp --output-dir ./diagrams`. This starts the TerraVision MCP server on your own computer.
- **Downloads:** on first start, `uvx` downloads the pinned `terravision` package and its Python dependencies from PyPI. Nothing else is fetched by the plugin.
- **Writes:** diagram files into the `diagrams` folder of the project you are working in.
- **For Terraform diagrams only:** TerraVision runs `terraform init` and `terraform plan` in the folder you point it at. Terraform then contacts whatever providers and module sources your code declares, using your own credentials, exactly as it would if you ran it yourself.
- **Fetches for Terraform diagrams:** if you give it a Git URL, or your Terraform uses remote modules, TerraVision downloads those sources, as Terraform itself would.
- **Edit in draw.io:** if you choose to open a diagram in draw.io's web app, your browser opens `app.diagrams.net` with the diagram carried in the part of the address after `#`, which browsers do not send to the server.
- **Sends:** TerraVision has no telemetry and does not upload your code or your diagrams anywhere.

## Privacy

TerraVision collects no data. Rendering happens on your computer. The conversation you have with Claude is handled by Anthropic under its own terms, as with any other Claude conversation.

## Source, issues and licence

- TerraVision source code and issue tracker: https://github.com/patrickchugh/terravision
- Documentation: https://patrickchugh.github.io/terravision/
- Security reports: see [SECURITY.md](SECURITY.md)
- Licence: AGPL-3.0-only, see [LICENSE](LICENSE)

This repository holds only the Claude plugin. The pinned TerraVision version here is updated with each TerraVision release.
