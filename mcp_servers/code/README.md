# Code Execution MCP Server

A sandboxed environment for executing shell commands and Python code with pre-installed scientific computing packages.

## Tools

### `code_exec`

Execute shell commands in a sandboxed bash environment.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `action` | `"help"` \| `"exec"` | `"exec"` | Action: 'help' for usage info, 'exec' to run code |
| `code` | string \| null | null | Shell command to execute (required for exec action) |

**Examples:**

```bash
# Run a simple command
code_exec(action="exec", code="echo 'Hello World'")

# Run Python with pre-installed packages
code_exec(action="exec", code="python -c 'import numpy as np; print(np.array([1,2,3]).mean())'")

# Get help
code_exec(action="help")
```

## Pre-installed Packages

The base environment includes:

**Data Science & Analytics:**
- numpy, pandas, scipy, statsmodels
- matplotlib, seaborn, plotly
- scikit-learn, xgboost
- duckdb

**Document Processing:**
- reportlab, fpdf2, pypdf, pdfplumber (PDF)
- openpyxl (Spreadsheets)
- python-pptx (Presentations)
- python-docx (Documents)
- beautifulsoup4, html5lib (HTML parsing)

**Finance:**
- numpy-financial (IRR, NPV, PMT)
- yfinance (Yahoo Finance data)
- pandas-datareader

**Additional:**
- pymupdf, PyPDF2, mpmath

## Optional Dependency Modules

Additional specialized packages can be enabled via build parameters in `arco.toml`:

### Medicine Module

Enable medical imaging and DICOM support with `INSTALL_MEDICINE=true`.

**Included packages:**
- pydicom (DICOM file format)

**Use case:** Medical image analysis, DICOM file processing, radiology data workflows.

### Scientific Computing Module

Enable computational biology and physics with `INSTALL_SCICOMP=true`.

**Included packages:**
- biopython (biological computation, sequence analysis)
- openmm (molecular dynamics simulation)
- pyhmmer (HMMER sequence search)
- particle (particle physics data)

**Use case:** Bioinformatics, molecular modeling, computational chemistry, particle physics research.

### Configuration

Set build parameters in `arco.toml`:

```toml
[arco.env.build]
INSTALL_MEDICINE = "false"  # Set to "true" to enable
INSTALL_SCICOMP = "false"   # Set to "true" to enable
```

Both modules can be enabled simultaneously. The install task automatically includes the selected dependency groups during build.
