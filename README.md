# PDFsearcher
<body>By using Python, I created a script that will search for strings in folder of PDFs.</body>

<h2>Why did I create this project?</h2>
<body>I was tasked with ensuring certain strings were present or not present in PDFs sent by a client. Due to the number of PDFs that needed to be filtered daily and the possibility of making mistakes in searching for these keywords, I created the script to automate the process. This cutdown thsi task to a couple of minutes and completely eliminated any escapes.</body>

<h2>Step 1: Import Modules</h2>
<ol>
  <li></li>
<pre><code>import os
import pdfplumber</code></pre>

</ol> 
def search_string_in_pdf(pdf_path, search_strings):
    """
    Search for strings in a PDF file.
    
    :param pdf_path: Path to the PDF file
    :param search_strings: List of strings to search for
    :return: Dictionary with the search strings as keys and boolean as values indicating if they were found
    """
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

def search_in_pdfs(directory, search_strings):
    """
    Search for strings in all PDF files in a directory.
    
    :param directory: Directory containing PDF files
    :param search_strings: List of strings to search for
    """
    results = {}
    for filename in os.listdir(directory):
        if filename.lower().endswith('.pdf'):
            pdf_path = os.path.join(directory, filename)
            results[filename] = search_string_in_pdf(pdf_path, search_strings)
    
    return results

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
