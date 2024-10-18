# PDFsearcher
<body>By using Python, I created a script that will search for strings in folder of PDFs.</body>

<h2>Why did I create this project?</h2>
<body>I was tasked with ensuring certain strings were present or not present in PDFs sent by a client. Due to the number of PDFs that needed to be filtered daily and the possibility of making mistakes in searching for these keywords, I created the script to automate the process. This cutdown thsi task to a couple of minutes and completely eliminated any escapes.</body>

<h2>Step 1: Import Modules</h2>
<body>PDFplumber is able to extract specified strings or data in PDFs.</body>
<pre><code>import os
import pdfplumber</code></pre>

</ol>

<h2>Step 2: Defined search for strings in a PDF file.</h2>
<body>This will define a function called search_string_in_pdf. It will search for specific strings within a PDF file and checks if they are present in the document.</body>
<ol>
  <li>Parameters:</li>
    <ul>
      <li>pdf_path: The path to the PDF file to be searched</li>
      <li>search_strings: A list of strings that you want to search within the PDF</li>
    </ul>
  <li>Dictionary Initilization (found_strings):</li>
    <ul>
      <li>A dictionary named found_strings is created with each string in search_strings as the key and False as the initial value. This dictionary will be used to track whether each string is found (True) or not (False) in the PDF.</li>
    </ul>
  <li>try-except Block:</li>
    <ul>
      <li>try: Attempts to open the PDF file using pdfplumber.open(). If there is an issue (e.g., file not found), the except block will handle the error.</li>
    </ul>
  <li>PDF Pages Loop:</li>
    <ul>
      <li>for page in pdf.pages: Loops through each page in the PDF.</li>
      <li>extract_text(): Extracts text from each page. If text is present, it converts the text to lowercase (text.lower()), which ensures case-insensitive searching.</li>
    </ul>
  <li>String Matching:</li>
    <ul>
      <li>For each string in search_strings, it checks if the lowercase version of the string is present in the lowercase text of the page.</li>
      <li>If the string is found, the corresponding value in found_strings is updated to True.</li>
    </ul>
  <li>Error Handling (except):</li>
    <ul>
      <li>If there is an error (e.g., file can't be opened), the function prints a message with the error details, including the pdf_path and the error message (e).</li>
    </ul>
  <li>Return Value:</li>
    <ul>
      <li>After processing all pages, the function returns the found_strings dictionary, indicating which strings were found (True) or not found (False).</li>
    </ul>
</ol>

<pre><code>
def search_string_in_pdf(pdf_path, search_strings):
    found_strings = {string: False for string in search_strings}
    
    try:
        with pdfplumber.open(pdf_path) as pdf:
            for page in pdf.pages:
                text = page.extract_text()
                if text:
                    text = text.lower()
                    for string in search_strings:
                        if string.lower() in text:
                            found_strings[string] = True
    except Exception as e:
        print(f"Error reading {pdf_path}: {e}")
    
    return found_strings 
  </code></pre>
  
<h2>Step 3: Search for strings in all PDF files in a dictionary</h2>
<body>This code defines a function search_in_pdfs that searches for specific strings across multiple PDF files within a given directory. It uses the previously defined function search_string_in_pdf to search within each individual PDF.</body>

<ol>
  <li>Parameters:</li>
    <ul>
      <li>directory: The path to the directory where PDF files are located.</li>
      <li>search_strings: A list of strings that you want to search within the PDF</li>
    </ul>
  <li>Initialize results Dictionary</li>
    <ul>
      <li>An empty dictionary results is initialized. This will be used to store the search results for each PDF file. The filename will be the key, and the search results (a dictionary of found strings) will be the value.</li>
    </ul>
  <li>Loop Through Files in the Directory</li>
    <ul>
      <li>os.listdir(directory) returns a list of all files and directories within the given directory.</li>
      <li>The function loops through each filename in that list.</li>
    </ul>
  <li>PDF Pages Loop:</li>
    <ul>
      <li>for page in pdf.pages: Loops through each page in the PDF.</li>
      <li>extract_text(): Extracts text from each page. If text is present, it converts the text to lowercase (text.lower()), which ensures case-insensitive searching.</li>
    </ul>
  <li>String Matching:</li>
    <ul>
      <li>For each string in search_strings, it checks if the lowercase version of the string is present in the lowercase text of the page.</li>
      <li>If the string is found, the corresponding value in found_strings is updated to True.</li>
    </ul>
  <li>Error Handling (except):</li>
    <ul>
      <li>If there is an error (e.g., file can't be opened), the function prints a message with the error details, including the pdf_path and the error message (e).</li>
    </ul>
  <li>Return Value:</li>
    <ul>
      <li>After processing all pages, the function returns the found_strings dictionary, indicating which strings were found (True) or not found (False).</li>
    </ul>
</ol>

<pre><code>
def search_in_pdfs(directory, search_strings):
    results = {}
    for filename in os.listdir(directory):
        if filename.lower().endswith('.pdf'):
            pdf_path = os.path.join(directory, filename)
            results[filename] = search_string_in_pdf(pdf_path, search_strings)
    
    return results

</code></pre>

<pre><code>
def main():
    directory = 'C:\\Users\\'  # Change this to your directory containing PDFs
    search_strings = ['ABC', 'DEF', 'HIJ', 'JKL']  # Change this to the list of strings you want to search for
    
    results = search_in_pdfs(directory, search_strings)
    
    for filename, found_strings in results.items():
        print(f"\nResults for {filename}:")
        for string, found in found_strings.items():
            print(f"  '{string}': {'Found' if found else 'No'}")

if __name__ == '__main__':
    main()

</code></pre>
