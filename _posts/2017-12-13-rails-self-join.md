---
layout: post
title: "Rails Self join explained"
date: 2017-12-13
---
Here in your migrations/schema, you will add a references column to the model itself.

class CreateUsers < ActiveRecord::Migration[5.0]
  def change
    create_table :users do |t|
      t.references :parent, index: true
      t.timestamps
    end
  end
end

class User < ApplicationRecord
  has_many :children ,class_name: "User", foreign_key: "parent_id"
  has_many :grandchildren, :through => :children, :source => :children
  belongs_to :parent, class_name: "User"
end
