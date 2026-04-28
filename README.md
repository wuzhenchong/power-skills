# Power Simulation Skills

This repository is a skill library for power simulation and power-system calculation workflows.

The current first skill is a MATPOWER 7.1 skill for Codex/Claude-style agents. Future work is expected to add more simulation-oriented skills, including a pandapower skill, while keeping each skill isolated under its own directory.

## Current Layout

```text
skills/
  matpower-71/
    SKILL.md                 # Official English skill entry point
    agents/openai.yaml       # Skill UI metadata
    references/*.md          # Official English progressive references

translations/
  matpower-71/
    SKILL.zh-CN.md           # Chinese translation for review and editing
    references/*.md          # Chinese translated reference materials

doc/
  similar-work-inspirations.md  # Notes from related power-system skill projects
```

The publishable MATPOWER skill is `skills/matpower-71`. Chinese files are maintenance aids and are intentionally kept outside the official skill directory.

## Roadmap

- Maintain and improve the MATPOWER 7.1 skill.
- Add a pandapower skill for Python-based power-system analysis.
- Keep future skills under `skills/<skill-name>/`.
- Keep translations and review materials under `translations/<skill-name>/`.
- Preserve progressive loading: concise `SKILL.md` files with detailed topic references.

## MATPOWER 7.1 Skill Goals

- Guide MATLAB/Octave programming with MATPOWER 7.1.
- Cover common MATPOWER functions and workflows through progressive references.
- Support source-level understanding of MATPOWER 7.1 when needed.
- Make the source modification boundary explicit: do not edit the user's installed MATPOWER package; edit project-local copies only.

## Similar Work

- [BenchFlow SkillsBench `ac-branch-pi-model`](https://github.com/benchflow-ai/skillsbench/tree/main/tasks/energy-ac-optimal-power-flow/environment/skills/ac-branch-pi-model) is a narrow, math-heavy skill for AC branch pi-model equations, MATPOWER branch fields, transformer taps, phase shifts, MVA limits, and sign/unit debugging.
- [Power-Agent PowerSkills](https://github.com/Power-Agent/PowerSkills) is a broader power-system skill collection that separates software workflows from mitigation playbooks. It covers tools such as pandapower, PyPSA, PSSE, PowerWorld, OpenDSS, ANDES, Egret, PSLF, LTSpice, and surge, and emphasizes the ladder: load/open -> inspect -> solve base case -> modify -> advanced studies.

See [doc/similar-work-inspirations.md](doc/similar-work-inspirations.md) for concise lessons these projects suggest for this repository.

## Official MATPOWER Sources

- MATPOWER GitHub: https://github.com/MATPOWER/matpower
- MATPOWER 7.1 User's Manual: https://matpower.org/docs/MATPOWER-manual-7.1.pdf
- MATPOWER 7.1 function reference: https://matpower.org/docs/ref/matpower7.1/lib/menu7.1.html
