# Pythonic HAML

This is an implementation of [HAML](http://haml-lang.com/) for Python.

I have kept as much of the same syntax as I could, but there are some Rubyisms built into HAML that I simply cannot replicate. I have tried to stay true to the original features while adapting them to be Pythonic.

This package essentially cross-compiles PyHAML code into a [Mako](http://www.makotemplates.org/) template. Ergo, all of your standard Mako syntax also applies to content which does not match any of the HAML syntax.

## API Example

    import haml
    import mako.template

    # 1. Write your HAML.
    haml_source = '.content Hello, World!'

    # 2. Parse your HAML source into a node tree.
    haml_nodes = haml.parse_string(haml_source)

    # 3. Generate Mako template source from the node tree.
    mako_source = haml.generate_mako(haml_nodes)

    # 4. Render the template.
    print mako.template.Template(mako_source).render_unicode()

Output:

    <div class="content">Hello, World!</div>
    
## Reference

Herein lies our differences to the [HAML reference](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html).

### [Attributes: ()](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html#attributes)

Python syntax must be used as if calling a function that takes keyword arguments. Eg.:

    %ul
        - for i in range(5):
            %li(id=['item', str(i)]) ITEM ${i}
    
renders to:
    
    <ul>
        <li id="item_0">ITEM 0</li>
        <li id="item_1">ITEM 1</li>
        <li id="item_2">ITEM 2</li>
        <li id="item_3">ITEM 3</li>
        <li id="item_4">ITEM 4</li>
    </ul>

You can also pass in mapping objects as positional objects. Eg.:

    - attrs = dict(a='one', b='two')
    %a(attrs)/

renders to:
    
    <a a="one" b="two" />

### [Python Evaluation: =](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html#ruby_evaluation)
### [Running Python: -](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html#running_ruby_)

Clearly this is now evaluating Python. It is evaluated in the Mako runtime context.

### [Python Interpolation: ${}](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html#ruby_interpolation_)

We are using Mako to do the heavy lifting here.

### [Filters](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html#filters)

We don't supply any filters, but the mechanism is there to take callables from the runtime globals to use as a filter. Eg.:

    -! def to_upper(x):
        return x.upper()
    :to_upper
        %p The syntaxes, they do nothing!
        #id x
        .class x
        - statement
        / comment

### [Boolean Attributes](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html#boolean_attributes)
### [Object Reference: []](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html#object_reference_)
### [Doctype: !!!](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html#doctype_)
### [Whitespace Preservation](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html#tilde)
### [Helpers](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html#helpers)

Haven't gotten around to these yet... I'm not really sure if I can do the object references anyways