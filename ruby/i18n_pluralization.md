# Using the Internationalisation (I18n) library to handle plurals

Every so often in our Rails views, we want to specify a quantity of items, such
as `3 apples` or `5 bananas`.

Often we hard code that text in our view:

```erb
<%= @fruit.count %> apples
```

And then we find that it looks wrong when we have just one item (`1 apples`). So
we may start to introduce variations:

```erb
<%= @fruit.count %> apple(s)  

<% if @fruit.count == 1 %>
  1 apple
<% else %>
  <%= @fruit.count %> apples
<% end %>

<%= pluralize(@fruit.count, 'apple') %> # using Rails' ActionView helpers
```

But what if we want to handle the case where we have no items differently? The
`pluralize` option above gives us `0 apples`, which is grammatically correct –
but what if we want `No apples` or just `—`?

The internationalisation library `i18n` allows us to abstract these decisions
away. If you provide an interpolation variable named `count`, you can
specify each preferred form in the `locales/XX.yml` file:

```erb
# in your view
<%= t('fruit.apple', count: @fruit.count) %>
```
```yaml
# in config/locales/en.yml
en:
  i18n:
    fruit:
      apple:
        zero: no apples
        one: an apple
        other: %{count} apples
```

The rules for which subkey to use depends both on the value of `count` and the
locale language itself. For instance, in English, the `zero` key is optional
and defaults to the `other` plural form if it doesn't exist. However, some
other languages have different plural forms for two, a few, or many items.

With the rules for which form to take abstracted away into the `i18n` library,
our views become language independent and free from unnecessary branching.

## Further reading

* [Pluralization in the Ruby on Rails Guides](http://guides.rubyonrails.org/i18n.html#pluralization)
* [Unicode Common Locale Data Repository's guide to plural rules](http://cldr.unicode.org/index/cldr-spec/plural-rules)
