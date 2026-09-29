# iron-oxide

Learning Rust, the code (not Fe₂O₃·nH₂O)

## CODE SAMPLE
```
use std::fmt;

// Define a trait (similar to an interface in other languages)
pub trait Summary {
    fn summarize(&self) -> String;
}

// Define a struct with ownership of its fields
#[derive(Debug)]
pub struct Article {
    pub headline: String,
    pub author: String,
    pub word_count: u32,
}

// Implement the Summary trait for Article
impl Summary for Article {
    fn summarize(&self) -> String {
        format!("'{}' by {} ({} words)", self.headline, self.author, self.word_count)
    }
}

// Define a custom error enum
#[derive(Debug)]
pub enum ArticleError {
    EmptyHeadline,
    InvalidWordCount,
}

// Implement Display for our custom error
impl fmt::Display for ArticleError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            ArticleError::EmptyHeadline => write!(f, "Headline cannot be empty"),
            ArticleError::InvalidWordCount => write!(f, "Word count must be greater than zero"),
        }
    }
}

// Constructor-like function returning a Result
impl Article {
    pub fn new(headline: &str, author: &str, word_count: u32) -> Result<Self, ArticleError> {
        if headline.trim().is_empty() {
            return Err(ArticleError::EmptyHeadline);
        }
        if word_count == 0 {
            return Err(ArticleError::InvalidWordCount);
        }

        Ok(Article {
            headline: headline.to_string(),
            author: author.to_string(),
            word_count,
        })
    }
}

fn main() {
    // 1. Creating a valid article using pattern matching on Result
    match Article::new("Understanding Rust Ownership", "Ferris", 1200) {
        Ok(article) => {
            // Passing a reference (&article) to borrow without taking ownership
            print_summary(&article);
        }
        Err(e) => println!("Failed to create article: {e}"),
    }

    // 2. Handling an invalid article creation
    if let Err(e) = Article::new("", "Anonymous", 500) {
        println!("Expected error caught: {e}");
    }
}

// Generics and Trait Bounds: accepts any type that implements Summary
fn print_summary<T: Summary>(item: &T) {
    println!("Summary: {}", item.summarize());
}
```
