# Tables of Contents for Well Trained Mind Press PDFs.

The PDFs I've purchased from [The Well Trained Mind](https://welltrainedmind.com/store/)
don't come with a table of contents.

You can add them yourself with the tools from [pdf.tocgen](https://krasjet.com/voice/pdf.tocgen/).

# Which ones are provided so far.

- Story of the World (25th edition).
    - Volume 1
        - [Main book](story-of-the-world-25th-1.toc)
        - [Instructor Guide](story-of-the-world-25th-1-instructor.toc)
        - [Tests and Answer Key](story-of-the-world-25th-1-tests.toc)
        - [Student's Book](story-of-the-world-25th-1-student.toc)
- Ordinary Parent's Guide to Teaching Spelling (Revised edition).
    - [Student Book](ordinary-parents-guide-to-teaching-reading-revised-student.toc)
    - [Instructor Book](ordinary-parents-guide-to-teaching-reading-revised-instructors.toc)
- First Language Lessons
    - [Level 1](first-language-lessons-1.toc)
- Writing With Ease
    - [Level 1](writing-with-ease-1.toc)
- Exploring the World Through Story (from Stonesoup Press, not Well Trained Mind)
    - [Level A](exploring-the-world-through-story-a.toc)

# The fast version.

I've already provided tables of contents for many books. You can add them to your PDF by:

- Clone this repository.
- Run ```uv run pdftocio YOUR.pdf < table_of_contents```

For instance:
- ```uv run pdftocio "Story of the World (25th edition).pdf" < sotw_toc```
- ```uv run pdftocio "OPG Revised Instructor Book.pdf" < opg_instructor_toc```
- ```uv run pdftocio "OPG Revised Student Book.pdf" < opg_student_toc```

# To do it yourself.

The instructions at [pdf.tocgen](https://krasjet.com/voice/pdf.tocgen/) are
reasonably straightforward.

First you need to build a "recipe" for how it can detect sections and subsections.
The easiest way to do this is point ```pdfxmeta``` to a page in the text containing
a section heading.

```
pdfxmeta --auto 1 --page 14 opg.pdf "Section" >> recipe.toml
```

Then you'll want to look at the resulting recipe and edit to taste.

Repeat as desired for subsections, chapters, etc.

```
pdfxmeta --auto 2 --page 28 opg.pdf "Lesson" >> recipe.toml
```

Then this recipe is used to find the actual page numbers in the PDF for
sections and chapters.

```
pdftocgen opg.pdf < recipe.toml > toc
```

The result is a simple text file of the format:

```
"Part 1 The Lessons" 24
"Section 1 Short-Vowel Sounds" 28
    "Lesson 1: The Vowel A a" 28
    "Lesson 2: The Vowel E e" 31
    "Lesson 3: The Vowel I i" 34
    "Lesson 4: The Vowel O o" 37
    "Lesson 5: The Vowel U u" 40
"Section 2 Consonant Sounds" 44
    "Lesson 6: The Consonant B b" 44
    "Lesson 7: The Consonant C c" 47
    "Lesson 8: The Consonant D d" 50
    "Lesson 9: The Consonant F f" 53
```

From the documentation of pdf.tocgen:

> The output of pdftocgen is a dialect of CSV. The main differences are
>
>    Every 4 spaces of indent represents a new heading level so a level 3 heading is 8 spaces of indent
>
>    The separator is a single space, not comma
>
>    Titles need to be quoted using double quotes.
>
> This format is intentionally designed to be easily edited (in Vim), since the output of pdftocgen is expected to be
> inaccurate in many cases and you are likely to tweak the table of contents before you import it to the original PDF file.

Again, you can edit this to taste.

Once you're happy with the table of contents, apply it to the PDF:

```
pdftoio opg.pdf < toc
```

This will not alter the original file but will create a new PDF with a table of contents applied.