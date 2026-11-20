# SPDX-FileCopyrightText: 2021-2026 Franklin Diaz <franklin@bitsmasher.net>
#
# SPDX-License-Identifier: MIT

ACLOCAL_AMFLAGS = -I config/m4 -I aclocal/latex-m4

.PHONY: check check-env lint clean dist-clean

check:
	@echo "Checking Stash House dependencies..."
	@which pass > /dev/null || echo "Warning: 'pass' command not found in PATH"
	@which gpg > /dev/null || echo "Warning: 'gpg' command not found in PATH"

lint:
	@npx -y markdownlint-cli "**/*.md" -c .markdownlint.json

clean:
	rm -rf *.egg-info __pycache__ .pytest_cache
	find . -name '*.pyc' -delete
	find . -name '__pycache__' -delete

dist-clean: clean
	rm -rf _build
