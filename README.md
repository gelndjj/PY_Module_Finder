# PY Module Finder

### CODE
Copy/Paste the code below modifying the sixth line "filepath" by your script file path.</br></br>
The code will parse your script file and write the require modules into a text file named requirements.txt.</br>

```
import pkg_resources
import importlib.util
import sys

# Set the path to the Python file you want to analyze
filepath = "PATH/example.py"

# Get a list of all the modules imported by the specified file
modules = set()
spec = importlib.util.spec_from_file_location("module.name", filepath)
module = importlib.util.module_from_spec(spec)
data = spec.loader.get_data(filepath)
if sys.version_info.major == 2:
    for line in data.splitlines():
        if line.startswith("import") or line.startswith("from"):
            words = line.split()
            if words[0] == "import":
                modules.add(words[1])
            else:
                modules.add(words[1].split(".")[0])
else:
    for line in data.splitlines():
        line = line.decode()
        if line.startswith("import") or line.startswith("from"):
            words = line.split()
            if words[0] == "import":
                modules.add(words[1])
            else:
                modules.add(words[1].split(".")[0])

# Get a list of all installed modules and their version numbers
installed_packages = pkg_resources.working_set
installed_packages_list = sorted(["%s==%s" % (i.key, i.version) for i in installed_packages if i.key in modules])

# Create the list of installed modules and their version numbers in a txt file at root
with open("requirements.txt","w") as file:
    for package in installed_packages_list:
        file.write(f"{package}\n")
```