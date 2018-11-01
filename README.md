### PushType
---
http://www.pushtype.org/

```ruby
class Article < PushType::Node
  field :body, :wysiwyg, vaidates: { presence: true }
  field :tags, tag_list
  field :author, :select, choices: -> { Author.all.map { |a| [a.name, a.id] } }
end


```

```ruby
config.root_nodes = [:home_page, :page]
config.home_slug = 'blog'
config.unexposed_nodes = [:testimonial, :author_profile]

class Testimonial < PushType::Node
  unexpose!
end

config.modia_styles = {
  large: '1024x1024>',
  medium: '512x512>',
  small: '256x256>'
}

image.media(:medium)

config.dragonfly_datastore = :s3
config.dragonfly_datastore_options = {
  bucket_name: ENV['S3_BUCKET'],
  access_key_kd: ENV['S3_ACCESS_KEY_ID'],
  secret_access_key: ENV['SECRET_ACCESS_KEY_ID']
}

```

```
rails g push_type:node page desciption:string body:wysiwyg

```

```ruby
class Page < PushType::Node
  has_child_nodes :all
  field :description, :string
  field :body, :wysiwyg
end

class Blog < PushType::Node
  has_child_nodes :article, order: :blog
end

class Article < PushType::Node
  field :except, :text
  field :body, :wysiwyg, validates: { presence: true }
  field :testimonial_ids, :relation, multiple: true
  field :tags, :tag_list
end

class Testimonial < PushType::Node
  unexpose!
end


class Person < PushType::Node
  filed :biography, :wysiwyg, validates: { presence: true }
  filed :date_of_birth, :date
end
me = Person.new tilte: 'Tky', date_of_birth: '1994-03-23'
me.title
me.date_of_birth
me.valid?


class Page < PushType::Node
  field :basic_1, :string
  field :basic_2, :text
  field :basic_3, :wysiwyg
  field :basic_4, :markdown
  field :basic_5, :number
  field :basic_6, :boolean
  field :basic_7, :date
  field :basic_8, :time
  
  field :sel_1, :select, chices: ['foo', 'bar', 'baz']
  field :sel_2, :select, chices: -> { Tag.all.map(&:name) }, multiple: true
  field :tags, :tag_list
  field :notes, :repeater, repeats: :text
  field :contacts, :matrix do
    field :name, :string
    field :age, :number
  end
  
  field :article_id, :relation
  field :foo_id, :relation, to: :article, scope: -> { published }
  field :image_id, :asset
  field :person, :structure do
    field :name, :string
    field :age, :number
    def adult?
      age && age >= 18
    end
  end
end

```

