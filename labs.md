<h1> Practical Threat Modeling with MITRE ATT&CK </h1>

# Demo 1: AI-Assisted Attack Tree from a DFD

- Open claude.ai, paste the prompt in [Live Demo](HACME_Cats_Live_Demo.md)
- Review the output[Threat Model](HACME_Cats_Threat_Model.html)

# Lab 1: Threat Modeling - Am I a Target?

Follow along using the MITRE ATT&CK Navigator https://mitre-attack.github.io/attack-navigator/enterprise/#

# Demo 2: Use an LLM to Augment ATT&CK with Fresh CTI

- Step 1: Export the baseline. Download Akira Navigator layer JSON from attack.mitre.org/groups/G1024
[Akira JSON](G1024-enterprise-layer.json)
- Step 2: Feed the blog to an LLM. "Parse this Arctic Wolf blog. Extract every ATT&CK technique. Return as JSON."
[AWN JSON](AWN_Akira_SonicWall_ATTCK_Techniques.json)
- Step 3: Merge into Navigator layer. "Add these techniques to my Navigator JSON with a different color. Output merged layer."
- Result: MITRE baseline (blue), Arctic Wolf CTI (orange), overlap (purple).
[Result](Akira_G1024_MITRE_plus_ArcticWolf_CTI_Merged.json)

# Lab 2: Cyballistics – Analyze the Adversary's Arsenal

- Clone at github.com/BishopFox/sliver
- Point Claude Code at github.com/BishopFox/sliver. 
- "Give me an overview of this C2 framework's architecture"
- "What ATT&CK techniques does Sliver implement? Map to source files"
- Pick one technique; ask Claude to walk through the code
- Document Technique ID | Source File | Behavior | Detection Opportunity

# Lab 3 - Threat Modeling – Visibility & Coverage with Think Red Act Blue ATT&CK Lens

Slicing and Dicing ATT&CK with Think Red Act Blue ATT&CK Lens

Created by Ismael Valenzuela and maintained under the new Think Red Act Blue platform.

Follow along using https://lens.thinkredactblue.com



