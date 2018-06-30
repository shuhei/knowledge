# make

## `Makefile`

### Indentation

Indentation has to be tabs. Have `autocmd FileType make setlocal noexpandtab` in `.vimrc`.

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
