# Makefile dla projektu LangGraph
# Zmienne konfiguracyjne
PYTHON = python3
VENV_NAME = venv
VENV_DIR = $(VENV_NAME)
VENV_PYTHON = $(VENV_DIR)/bin/python
VENV_PIP = $(VENV_DIR)/bin/pip
REQUIREMENTS = requirements.txt

# Domyślny cel
.DEFAULT_GOAL := help

# Phony targets (cele, które nie tworzą plików)
.PHONY: help venv install install-dev clean test lint run

# Pomoc - wyświetla dostępne komendy
help:
	@echo "Dostępne komendy:"
	@echo "  make venv        - Tworzy środowisko wirtualne"
	@echo "  make install     - Instaluje podstawowe zależności"
	@echo "  make install-dev - Instaluje wszystkie zależności (dev + prod)"
	@echo "  make clean       - Usuwa środowisko wirtualne"
	@echo "  make test        - Uruchamia testy"
	@echo "  make lint        - Uruchamia linter"
	@echo "  make run         - Uruchamia aplikację"

# Tworzenie środowiska wirtualnego
$(VENV_DIR)/bin/activate: requirements.txt
	test -d $(VENV_DIR) || $(PYTHON) -m venv $(VENV_DIR)
	$(VENV_PIP) install --upgrade pip
	touch $(VENV_DIR)/bin/activate

venv: $(VENV_DIR)/bin/activate

# Instalacja podstawowych zależności LangGraph
install: venv
	$(VENV_PIP) install --upgrade pip
	$(VENV_PIP) install langgraph
	$(VENV_PIP) install langchain-core
	$(VENV_PIP) install langchain-openai
	$(VENV_PIP) install python-dotenv
	$(VENV_PIP) install langgraph-cli
	@if [ -f $(REQUIREMENTS) ]; then $(VENV_PIP) install -r $(REQUIREMENTS); fi

# Instalacja zależności deweloperskich
install-dev: install
	$(VENV_PIP) install langgraph-sdk
	$(VENV_PIP) install langgraph-checkpoint-sqlite
	$(VENV_PIP) install langsmith
	$(VENV_PIP) install langchain-community
	$(VENV_PIP) install pytest
	$(VENV_PIP) install black
	$(VENV_PIP) install flake8

# Czyszczenie środowiska
clean:
	rm -rf $(VENV_DIR)
	rm -rf __pycache__
	rm -rf .pytest_cache
	find . -type f -name "*.pyc" -delete

# Uruchamianie testów
test: venv
	$(VENV_PYTHON) -m pytest tests/

# Formatowanie i linting kodu
lint: venv
	$(VENV_PYTHON) -m black .
	$(VENV_PYTHON) -m flake8 .

# Uruchamianie aplikacji
run: venv
	$(VENV_PYTHON) main.py

# Generowanie requirements.txt
freeze:
	$(VENV_PIP) freeze > requirements.txt

