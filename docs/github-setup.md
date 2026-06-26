# GitHub Setup

Suggested repository name:

```text
RTL-Linear-Regression-Accelerator
```

or:

```text
FPGA-Linear-Regression-Accelerator
```

## Push This Public Dossier

```bash
cd ~/Documents
unzip /path/to/RTL-LR-Accelerator-public.zip
mv RTL-LR-Accelerator-public RTL-Linear-Regression-Accelerator
cd RTL-Linear-Regression-Accelerator

git init
git branch -M main
git add README.md docs assets .gitignore
git commit -m "Create public RTL accelerator technical dossier"

git remote add origin https://github.com/an-thony350/RTL-Linear-Regression-Accelerator.git
git push -u origin main
```

## Files to Keep Public

- `README.md`
- `docs/`
- `assets/.gitkeep`
- `.gitignore`

## Files Not to Add

- private keys,
- bitstreams/HWH/TCL files,
- notebooks,
- Python trading code,
- cloud/API code,
- controller/audio code,
- RTL/HLS source,
- raw logs/data,
- credential/config files.

