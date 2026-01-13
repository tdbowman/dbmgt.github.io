# Introduction to Database Management

An interactive online textbook for IN 251 / IM 751 / LIS 751 at the School of Information Studies.

## About This Project

This textbook is built using [Jupyter Book](https://jupyterbook.org/) with [MyST Markdown](https://myst-parser.readthedocs.io/), combining traditional reading materials with interactive Jupyter Notebooks for hands-on SQL practice.

## Getting Started

### Prerequisites

- Python 3.9 or higher
- PostgreSQL 14 or higher (for local database exercises)
- Git (optional, for version control)

### Installation

1. Clone or download this repository:
   ```bash
   git clone https://github.com/yourusername/database-textbook.git
   cd database-textbook
   ```

2. Create a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

### Building the Book

To build the HTML version of the book:

```bash
# Standard build
jupyter-book build .

# Or force rebuild all pages
jupyter-book build . --all

# Alternative short command
jb build .
```

The built book will be in `_build/html/`. Open `_build/html/index.html` in your browser to view it.

**Troubleshooting:**

If you see errors about MyST-MD or unexpected options, you may have a newer version of jupyter-book. Try:

```bash
# Install the stable Sphinx-based version
pip install "jupyter-book>=0.15,<1.0"

# Then rebuild
jupyter-book build . --all
```

### Publishing to GitHub Pages

```bash
ghp-import -n -p -f _build/html
```

## Project Structure

```
database-textbook/
├── _config.yml          # Book configuration
├── _toc.yml             # Table of contents
├── intro.md             # Landing page
├── chapters/            # MyST Markdown chapters
│   ├── 00-setup.md
│   ├── 01-intro-databases.md
│   └── ...
├── notebooks/           # Interactive Jupyter notebooks
│   ├── 05-sql-basics-practice.ipynb
│   └── ...
├── appendices/          # Reference materials
│   ├── sql-cheat-sheet.md
│   ├── glossary.md
│   └── resources.md
├── data/                # Sample datasets
├── images/              # Figures and diagrams
├── _static/             # Custom CSS/JS
├── requirements.txt     # Python dependencies
└── references.bib       # Bibliography
```

## For Students

### Running the Notebooks Locally

1. Start Jupyter:
   ```bash
   jupyter lab
   ```

2. Navigate to the `notebooks/` directory and open any `.ipynb` file.

3. Make sure PostgreSQL is running and update the connection string with your password.

### Using Google Colab

Click the Colab button on any notebook page to run it in Google Colab (no local setup required).

## For Instructors

### Customizing Content

- Edit `.md` files in `chapters/` for written content
- Edit `.ipynb` files in `notebooks/` for interactive exercises
- Update `_toc.yml` to change chapter order
- Modify `_config.yml` for book settings

### Adding New Chapters

1. Create a new `.md` or `.ipynb` file
2. Add it to `_toc.yml`
3. Rebuild the book

## Technologies Used

- **Jupyter Book** - Book building framework
- **MyST Markdown** - Extended Markdown syntax
- **PostgreSQL** - Database for examples
- **ipython-sql** - SQL magic for Jupyter
- **Sphinx** - Documentation engine

## License

This textbook is licensed under [Creative Commons BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/).

## Contributing

Found an error or have a suggestion? Please open an issue or submit a pull request!

## Contact

Dr. Timothy D Bowman  
School of Information Studies  
[Course Canvas Site]
