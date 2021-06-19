# roact-hooks
An implementation of [React hooks](https://reactjs.org/docs/hooks-intro.html) for [Roact](https://github.com/Roblox/roact). Does not make any modifications to Roact itself.

## Example
```lua
local Hooks = require(ReplicatedStorage.Hooks)
local Roact = require(ReplicatedStorage.Roact)

-- `props` are our normal passed in properties.
-- `hooks` is passed in by roact-hooks itself.
local function Example(props, hooks)
	local count, setCount = hooks.useState(0)

	hooks.useEffect(function()
		print("the count is", count)
	end)

	return Roact.createElement(Button, {
		onClick = function()
			setCount(count + 1)
		end,

		text = count,
	})
end

-- This returns a component that you can call `Roact.createElement` with
Example = Hooks.new(Roact)(Example, "Example")
```

## Implemented Hooks

### useState
`useState<T>(defaultValue: T) -> (T, update: (newValue: T) -> void)`

Used to store a stateful value. Returns the current value, and a function that can be used to set the value.

### useEffect
`useEffect(callback: () -> (() -> void)?, dependencies?: any[])`

Used to perform a side-effect with a callback function.

This callback function can return a destructor. When the component unmounts, this function will be called.

You can also pass in a list of dependencies to `useEffect`. If passed, then only when those dependencies change will the callback function be re-ran.

## Rules of Hooks
The rules of roact-hooks are not quite the same as the ones in React. You can, for instance, call a hook in a conditional or a loop, since internally a counter is not used. Whether or not you *should* is another question, but it should technically work!

### Only call hooks from Roact functions.

You can only call hooks from:
- Roact function components
- Custom hooks (a function that begins with the word `use`)

### Only call one hook per line.

This is a roact-hooks specific inclusion. This is necessary due to the implementation of roact-hooks, and how it separates calls from each other--it checks the function that called it and the line number. That means two uses of `useState` or `useEffect` on one line will be confused for each other. This limitation can be lifted if a column attribute is added to `debug.info`.