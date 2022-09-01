# clone sage repo and move to sage dir
```bash
git clone https://github.com/blackducksoftware/sage.git
cd sage
```

# create python venv in the sage dir
```bash
python3 -m venv .
```

# activate venv
```bash
source bin/activate
```

# install py dependencies
```bash
pip3 install -r requirements.txt
```

# run sage
```bash
python3 sage.py https://your-hub-dns {api-token} -f sage_says.json
```

# check results (examples)
```bash
jq '.unmapped_scans' < sage_says.json
jq '.projects_with_too_many_versions' < sage_says.json
jq '.high_frequency_scans' < sage_says.json
```
