---
layout: post
title:  "Nil as a Hash Key"
date:   2015-10-02 15:08:13
categories: ruby
---
I was doing a little code review with a teammate and one of his hash definitions caught my eye.  It looked something like this...

{% highlight ruby %}
hsh = {
  :foo => Foo,
  :bar => Bar,
  nil  => Qux
}
{% endhighlight %}

This had nothing to do with our conversation, but... 'nil'? "Can you really use 'nil' as a Hash key?" I asked.  Apparently you can. After all ([as Sandi Metz taught us](https://youtu.be/OMPfEXIlTVE)) nothing IS something. So, I tried it, and sure enough, it works.  You can use the 'active nothing' in this way.

{% highlight ruby %}
k1 = method_that_might_return_a_symbol()
#=> :foo
hsh[k1].new
#=> #<Foo:0x007fc8b203b970>
key = method_that_might_return_a_symbol()
#=> nil
hsh[key].new
#=> #<Qux:0x007fc8b203b970>
{% endhighlight %}

Now, I'm not sure there is a huge use case for this, but it is cleaner than writing a condition to ensure the Hash has a value before calling it, so it might have some uses.

{% highlight ruby %}
hsh[key].new if hsh[key]
{% endhighlight %}


However, this doesn't let you get around passing a key.

{% highlight ruby %}
hsh[]
#=> ArgumentError: wrong number of arguments (0 for 1)
{% endhighlight %}

That is, unless you do this first...

{% highlight ruby %}
class Hash
  SETTER = Hash.instance_method(:[]=)
  GETTER = Hash.instance_method(:[])
  def []=(key=nil, value)
    SETTER.bind(self).call(key, value)
  end
  def [](key=nil)
    GETTER.bind(self).call(key)
  end
end
{% endhighlight %}

...but you should probably never do that.
