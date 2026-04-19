# LaTeX build: sources in src/, PDF and aux files in project root (XeLaTeX + biber).

LATEX = xelatex
BIBTEX = biber
MAKEINDEX = makeindex

MAIN = main
SRCDIR = src
PDFFILE = $(MAIN).pdf
LATEX_CD = cd $(SRCDIR) && $(LATEX) $(LATEXFLAGS) -output-directory=..

INPUTS = \
	$(SRCDIR)/metadata.tex \
	$(SRCDIR)/preamble.tex \
	$(SRCDIR)/fonts-local.tex \
	$(SRCDIR)/unir-style.tex \
	$(SRCDIR)/cover.tex \
	$(SRCDIR)/abstract.tex \
	$(SRCDIR)/indexes.tex \
	$(SRCDIR)/group-work.tex \
	$(SRCDIR)/introduction.tex \
	$(SRCDIR)/state-of-the-art.tex \
	$(SRCDIR)/methodology.tex \
	$(SRCDIR)/results.tex \
	$(SRCDIR)/conclusions.tex \
	$(SRCDIR)/appendix.tex

BUILDDIR = build
LATEXFLAGS = -interaction=nonstopmode -file-line-error -synctex=1

GREEN = \033[0;32m
RED = \033[0;31m
YELLOW = \033[1;33m
NC = \033[0m

.PHONY: all clean pdf validate help quick cleanall

all: pdf validate

pdf: $(PDFFILE)

$(PDFFILE): $(SRCDIR)/$(MAIN).tex $(INPUTS) $(SRCDIR)/references.bib
	@echo "$(YELLOW)=== Compiling LaTeX document ===$(NC)"
	@echo "$(YELLOW)First pass:$(NC)"
	@$(LATEX_CD) $(MAIN).tex || (echo "$(RED)ERROR: First LaTeX pass failed$(NC)" && exit 1)
	@echo "$(YELLOW)Bibliography:$(NC)"
	@$(BIBTEX) --input-directory=$(SRCDIR) $(MAIN) || (echo "$(YELLOW)WARNING: biber failed (ok if no citations)$(NC)")
	@echo "$(YELLOW)Index:$(NC)"
	@if [ -f $(MAIN).idx ]; then $(MAKEINDEX) $(MAIN) || true; fi
	@echo "$(YELLOW)Second pass:$(NC)"
	@$(LATEX_CD) $(MAIN).tex || (echo "$(RED)ERROR: Second LaTeX pass failed$(NC)" && exit 1)
	@echo "$(YELLOW)Third pass:$(NC)"
	@$(LATEX_CD) $(MAIN).tex || (echo "$(RED)ERROR: Third LaTeX pass failed$(NC)" && exit 1)
	@echo "$(GREEN)✓ PDF: $(PDFFILE)$(NC)"

validate: $(PDFFILE)
	@echo "$(YELLOW)=== Validating PDF ===$(NC)"
	@PDF_SIZE=$$(stat -f%z $(PDFFILE) 2>/dev/null || stat -c%s $(PDFFILE) 2>/dev/null); \
	if [ ! -f $(PDFFILE) ] || [ ! -s $(PDFFILE) ] || [ "$$PDF_SIZE" -lt 1000 ]; then \
		echo "$(RED)ERROR: invalid $(PDFFILE)$(NC)"; exit 1; \
	fi; \
	echo "$(GREEN)✓ OK ($$PDF_SIZE bytes)$(NC)"

quick: $(SRCDIR)/$(MAIN).tex
	@$(LATEX_CD) $(MAIN).tex || exit 1

clean:
	@rm -f *.aux *.bbl *.blg *.log *.out *.toc *.lof *.lot *.idx *.ind *.ilg *.synctex.gz
	@rm -f *.bcf *.run.xml missfont.log
	@rm -rf $(BUILDDIR)
	@echo "$(GREEN)✓ clean$(NC)"

cleanall: clean
	@rm -f $(PDFFILE)
	@echo "$(GREEN)✓ cleanall$(NC)"

help:
	@echo "Targets: all (default), pdf, validate, quick, clean, cleanall"
