# make

## `Makefile`

### Indentation

Indentation has to be tabs. Have `autocmd FileType make setlocal noexpandtab` in `.vimrc`.

### Function

- Use `$(call func_name,param1,param2)` to call a function. It returns an expression as is.
- To evaluate the expression as a shell command, use `$(shell your_command)` in the function.

For example:

```make
define get_container_id
	$(shell docker ps --format "{{.ID}}" --filter "ancestor=$(1)")
endef

docker-stop:
	docker stop $(call get_container_id,$(image_name))
```
