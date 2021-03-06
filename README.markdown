mongoid-rspec
=

RSpec matchers for Mongoid.

Association Matchers
-
    describe User do
      it { should reference_many(:articles).with_foreign_key(:author_id) }
      it { should have_many(:articles).with_foreign_key(:author_id) }
  
      it { should reference_one(:record) }
      it { should have_one(:record) }    
  
      it { should reference_many :comments }
      it { should have_many :comments }
  
      it { should embed_one :profile }
  
      it { should reference_and_be_referenced_in_many(:children).of_type(User) }
      it { should have_and_belong_to_many(:children) }
    end

    describe Profile do
      it { should be_embedded_in(:user).as_inverse_of(:profile) }
    end

    describe Article do
      it { should be_referenced_in(:author).of_type(User).as_inverse_of(:articles) }
      it { should belong_to(:author).of_type(User).as_inverse_of(:articles) }
      it { should embed_many(:comments) }
    end

    describe Comment do
      it { should be_embedded_in(:article).as_inverse_of(:comments) }
      it { should be_referenced_in(:user).as_inverse_of(:comments) }
    end

    describe Record do
      it { should be_referenced_in(:user).as_inverse_of(:record) }
    end

Validation Matchers
-
    describe User do
      it { should validate_presence_of(:login) }
      it { should validate_uniqueness_of(:login) }    
      it { should validate_format_of(:login).to_allow("valid_login").not_to_allow("invalid login") }
      it { should validate_associated(:profile) }
      it { should validate_inclusion_of(:role).to_allow("admin", "member") }
      it { should validate_numericality_of(:age) }
      it { should validate_length_of(:us_phone).as_exactly(7) }
      it { should validate_confirmation_of(:email) }
    end
    
    describe Article do
      it { should validate_length_of(:title).within(4..100) }
    end

Others
-
    describe User do
      it { should have_fields(:email, :login) }
      it { should have_field(:active).of_type(Boolean).with_default_value_of(false) }
      it { should have_fields(:birthdate, :registered_at).of_type(DateTime) }

      # useful if you use factory_girl and have Factory(:user) defined for User
      it { should save }
      
      it { should be_timestamped_document } # if you're declaring `include Mongoid::Timestamps`
      it { should be_versioned_document } # if you're declaring `include Mongoid::Versioning`
      it { should be_paranoid_document } # if you're declaring `include Mongoid::Paranoia`
    end

Use
-
add in Gemfile

    gem 'mongoid-rspec'
    
drop in existing or dedicated support file in spec/support (spec/support/mongoid.rb)

    RSpec.configure do |configuration|
      configuration.include Mongoid::Matchers
    end
    
Acknowledgement
-
Thanks to [Durran Jordan](https://github.com/durran) for providing the changes necessary to make 
this compatible with mongoid 2.0.0.rc, and for other [contributors](https://github.com/evansagge/mongoid-rspec/contributors) 
to this project.