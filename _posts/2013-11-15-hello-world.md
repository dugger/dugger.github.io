---
layout: post
title:  "Nil as a Hash Key"
date:   2015-10-02 15:08:13
categories: ruby
---
I was doing come code review with a team mate and something caught my eye.  It looked like this...

{% highlight ruby linenos %}
hsh = {
  :foo => Foo,
  :bar => Bar,
  nil  => Qux
}
{% endhighlight %}

This had nothing to do with our conversation, but, 'nil'...  can you really use 'nil' as a Hash key?  Apparently you can. After all ([as Sandi Metz taught us](https://youtu.be/OMPfEXIlTVE)) nothing is something. So, I tried it, and sure enough, it works.  You can use the 'active nothing' like this.

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

Not a huge use case, but its cleaner than writing a condition to ensure the Hash has a value, so it has some uses.  

{% highlight ruby %}
hsh[key].new if hsh[key]
{% endhighlight %}

However, this doesn't work.

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
