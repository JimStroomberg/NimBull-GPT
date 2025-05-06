 # NimBull-GPT
 
 This repository contains the configuration files and documentation needed to connect OpenAI GPT to the Bullhorn REST API via a public proxy.
 
 🧠 This project does not include the proxy itself — only the integration layer used by GPT.
 
 ---
 
 ## 📁 Contents
 
 | File                | Description                                         |
 |---------------------|-----------------------------------------------------|
 | `NimBull-GPT.yaml`  | OpenAPI 3.1 spec used by GPT to define the proxy    |
 | `gpt-actions.json`  | Action config that tells GPT how to authenticate    |
 | `gpt-helper.html`   | Full usage guide and examples for GPT to reference |
 
 ---
 
 ## 🎯 Purpose
 
 This repo is designed for use inside the GPT builder. It connects GPT to a public Bullhorn proxy with:
 
 - Smart field defaults per entity (e.g. Skill, Candidate)
 - Auto-pagination (no `start` or `count` needed)
 - `meta` support for introspection
 - Natural language-ready query interface
 
 ---
 
 ## 🌐 Hosting via GitHub Pages
 
 These files are intended to be publicly hosted so GPT can read them:
 
 - `NimBull-GPT.yaml` → The OpenAPI schema
 - `gpt-helper.html` → Usage examples and GPT guidance
 - `gpt-actions.json` → Manifest used in GPT Builder
 
 Example usage in `gpt-actions.json`:
 ```json
 "api": {
   "type": "openapi",
   "url": "https://github.com/nimbl-recruitment/NimBull-GPT/NimBull-GPT.yaml"
 }
 ```
 
 ---
 
 ## ✅ How to Use
 
 1. Deploy the proxy from the separate proxy repo (e.g. NimBull-GPT-Proxy)
 2. Host these files via GitHub Pages
 3. Upload `gpt-actions.json` in the GPT builder
 4. Paste the matching OpenAPI YAML when prompted
 
 GPT will now be able to talk to Bullhorn naturally.
 
 ---
 
 Made for non-technical users to access Bullhorn through AI ✨

 ## License
 
Copyright (C) Nimbl-recruitment B.V.
Unauthorized copying of this file, via any medium, is strictly prohibited.
Proprietary and confidential.
Written by Jim Stroomberg <j.stroomberg@nimbl-recruitment.com>, March 2025.