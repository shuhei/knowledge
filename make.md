# make

## `Makefile`

### References:

- https://swcarpentry.github.io/make-novice/reference.html

### Indentation

Indentation has to be tabs. Have `autocmd FileType make setlocal noexpandtab` in `.vimrc`.

### Targets

- The first target is treated as the default target (It can be changed by `.DEFAULT_TARGET := foo` as of Make 3.81)
- Use file names as targets if possible
- List up dependencies of a target. Dependencies can be files (targets or not) or phony targets. List up depended files to be able to re-build when they change (avoid `make: 'foo' is up to date`).
- Put `.PHONY` before non-file-name targets

```make
.PHONY: foo
foo: foo.c
	# Do something

.PHONY: bar
bar: bar.c
	# Do something
```

### Macro

We can organize reusable patterns in `Makefile` with macros.

#### Simple macros

```make
name=world

hello:
	echo $(name)
```

#### Macros with arguments

- Use `$(call func_name,param1,param2)` to expand a macro with arguments.
- To run the expression as a shell command, use `$(shell your_command)` in the macro.

For example:

```make
get_container_id=$(shell docker ps --format "{{.ID}}" --filter "ancestor=$(1)")
# or
define get_container_id
$(shell docker ps --format "{{.ID}}" --filter "ancestor=$(1)")
endef

docker-stop:
	docker stop $(call get_container_id,$(image_name))
```
