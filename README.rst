
FEDerated GENeral “Omics” (FEDGEN) HPC User Documentation
----------------------------------------------------------

Served via http://hpcdocs.fedgen.net

Project Team
================
Engr Boladele Akanle,
Mr Emmanuel AyoAriyo,
Prof Adetiba Emmanuel,
Mrs Mary Sweetwilliams,
Romiluyi Tolulope (Consultant),
Covenant University

License
===========
Text is licensed under [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/),
code examples are provided under the [MIT](https://opensource.org/licenses/MIT) license.


Locally building the HTML for testing
=========================================
```
git clone https://github.com/toluromio/fedgenhpc/fedgenhpc.git
cd fedgenhpc
virtualenv venv
source venv/bin/activate
pip install sphinx
pip install sphinx_rtd_theme
sphinx-build . _build
```

Then point your browser to `_build/index.html`.


