Compasse Rust user guide:

1: Exam Creation
  - Create the JSON exam file (test/source.json)
    This file describes the exam you want to generate

  - Modify the style file (test/style.typ)
    This file describes how your exam will look like

  - Disable the metadata (test/meta.json)
    Change the "active" field fo false

  - Run the first compilation
    $ typst/target/release/typst compile test/main.typ

  - Find the number of pages and the exam id
    this can be automated in the future! 
    the page number is found in test/dest/file.json at "md"."n"
    the exam id is found in test/dest/file.json at "md"."id". It is also defined by your own exam file.

  - Call generator to create the qr codes
    $ generator/target/release/generator -p=<page number> -e=<exam id> test/source.json test/style.typ

  - Copy the array returned by the program to the metadata (test/meta.json)
    paste it as the value of the field "hash"

  - Copy the contents of the qr folder (generator/tests/data/) to the typst exam file's folder (test/qr/)

  - Enable the metadata (test/meta.json)
    Change the "active" field fo true

  - Run the second compilation
    $ typst/target/release/typst compile test/main.typ

You now have your exam (test/main.pdf), and the files to go with it (test/dest/).
Do not loose them!
You can now print your exam, have it answered, and scan the answered papers.

2: Exam Notation

  - Scan your exam files. 
    You do not need to scan in color.
    A resolution of 300dpi is appreciated but not required. The notation should work even at 100dpi.
    Your scans must be of roughly the same size as your original document.
      that is, if your exam was on A4 paper, you should scan it as A4.
      to put it another way, the margins of the scanned files must be minimal.
    Your scans do not need to be upright. They can be slightly rotated.
      they can also be upside down.
      they cannot currently be rotated 90°, though this is something that can be fixed.
    Your scans must not be mirrored.

  - 


