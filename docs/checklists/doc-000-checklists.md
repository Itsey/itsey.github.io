# How To Create A Check-list.

## Pre-requisites

* Markdown Editor Installed

[meta]#Reference;
GhostWriter - Nice toggle on the markdown preview clean editing no folder view.
Visual Studio Addin - Clunky but works within Visual Studio.
MarkDown Monster - Big and clumsy, costs.
Markdown Editor - Neat but takes some getting used to, does not support comments properly.
[meta-end]

## Steps

[step]:

#### Step - 5 - Copy the template.

Copy the template file to the new filename.

```cmd
cd C:\files\code\github\itsey.github.io\docs\checklists\
```

```cmd
copy doc-100-template.md <newname>.md
```

[step]:

#### Step - 10 - H1 Create a Title

Using # H1 create a title for the check-list, prepare the sections pre-requisites and steps with H2 headings.

#### Step - 20 - Create a Meta Heading

```cmd
[meta-title]:#kata;#Level100;#AuthorJim;
[meta-review]:10/10/2024
```

[meta]#explanation;

Meta-title comments include overview information about the checklist.

Kata tag is a repeatable checklist.

Level tag is Level100,Level200,Level300 - describes the technical level of the content.
Author is the name of the author ( or other identity).

Meta-review information contains a date when the content was last reviewed for accuracy.

#### Step - 10 - H4 Create A Step.

Prior to the step insert a comment with the word step, no space.  Add tags using #word and a semicolon to separate.  Use H4 to highlight the step text.

```cmd
[step]:
```

When creating the step you can also add [meta] comments that can highlight areas of the step.

```cmd
[meta]:#Reference;#Beginner;
```

Tags can be 

 #Beginner - Shown only when beginner is shown 

#Advanced - Shown as advanced Step only 

Enter the text for the step.  This should be a paragraph but can contain any additional information. 

#### Step - 40 - Add step details

Within the step you can add [meta] to highligght additonal levels of infomation

Explanation - Additional Explanatory text

Reference - Additional reference links to other explanatory content
