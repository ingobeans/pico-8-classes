# pico-8-classes

basic classes implementation for pico-8.

# example usage

```lua
--define an 'animal' class
animal = class({
  health=10,
  name="",
  eat=function (this)
    print(this.name.." has eaten")
    this.health += 1
  end
})

--define a 'person' class
--which extends from animal
person = class({
  cash=100,
  talk=function (this)
    print(this.name..": yippidi dooo")
  end
},animal)

--create instance of animal
george = animal.new({
  name="george the dog"
})

--call method (pass self)
george.eat(george)

--create instance of person
ingo = person.new({name="ingo"})
--call method (pass self)
ingo.talk(ingo)
```

# minimum install (add this code snippet)

```lua
--pico-8 classes impl

--github
--https://github.com/ingobeans/pico-8-classes

function copy(t)
	--shallow copy table
	n = {}
	for k,v in pairs(t) do
		n[k] = v
	end
	return n
end

classes = {}
function class(t, extends)
	--function for defining
	--classes
	local id = #classes+1
	local c = extends or {
		new=function(t)
			local n = copy(classes[id])
			for k,v in pairs(t) do
				n[k] = v
			end
			n.new = nil
			return n
		end,
		class_id=id
	}
	for k,v in pairs(t) do
		c[k] = v
	end
	add(classes,c)
	return c
end
```

# optional add-on

```lua
--function that stringifies a table in full
--to make it printable

function dump(t,ind)
	--stringify table
	s = ""
	if ind == nil then
		s = s.."{\n"
	end
	local indl = ind or 1
	local	indent = ""
	for i=1,indl do
		indent = indent.." "
	end
	for k,v in pairs(t) do
		if type(v) == "table" then
			s = s..indent..k..":{\n"..dump(v,indl+1)..indent.."}\n"
		elseif type(v) == "string" then
			s = s..indent..k..":\""..tostr(v).."\"\n"
		else
			s = s..indent..k..":"..tostr(v).."\n"
		end
	end
	if ind == nil then
		s = s.."}"
	end
	return s
end
```
