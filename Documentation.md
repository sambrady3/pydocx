#pydocx
	pydocx is a parser that breaks down the elements of a docxfile and converts them
	into different markup languages. Right now, HTML is supported. Markdown and LaTex
	will be available soon. You can extend any of the available parsers to customize it
	to your needs. You can also create your own class that inherits DocxParser
	to create your own methods for a markup language not yet supported.
	
#Currently Supported
	tables
		nested tables
		rowspans
		colspans
		lists in tables
	lists
		list styles
		nested lists
		list of tables
		list of pragraphs
	justification
	images
	bold 
	italics
	underline
	hyperlinks
	headings


#Usage

	DocxParser includes abstracts methods that each parser overwrites to satsify its own 
	needs. The abstract methods are as follows:
	
		class DocxParser:
	
		@property
		def parsed(self):
			return self._parsed

		@property
		def escape(self, text):
			return text

		@abstractmethod
		def linebreak(self):
			return ''

		@abstractmethod
		def paragraph(self, text):
			return text

		@abstractmethod
		def heading(self, text, heading_level):
			return text

		@abstractmethod
		def insertion(self, text, author, date):
			return text

		@abstractmethod
		def hyperlink(self, text, href):
			return text

		@abstractmethod
		def image_handler(self, path):
			return path

		@abstractmethod
		def image(self, path, x, y):
			return self.image_handler(path)

		@abstractmethod
		def deletion(self, text, author, date):
			return text

		@abstractmethod
		def bold(self, text):
			return text

		@abstractmethod
		def italics(self, text):
			return text

		@abstractmethod
		def underline(self, text):
			return text

		@abstractmethod
		def tab(self):
			return True

		@abstractmethod
		def ordered_list(self, text):
			return text

		@abstractmethod
		def unordered_list(self, text):
			return text

		@abstractmethod
		def list_element(self, text):
			return text

		@abstractmethod
		def table(self, text):
			return text

		@abstractmethod
		def table_row(self, text):
			return text

		@abstractmethod
		def table_cell(self, text):
			return text

		@abstractmethod
		def page_break(self):
			return True

		@abstractmethod
		def indent(self, text, left='', right='', firstLine=''):
			return text
			
		
	Docx2Html inherits DocxParser and implements basic HTML handling. Ex.
		
	class Docx2Html(DocxParser):

		def escape(self, text):
			return xml.sax.saxutils.quoteattr(text)[1:-1] #  Escape '&', '<', and '>' so we 
														  #  render the HTML correctly
		def linebreak(self, pre=None):					  
			return '<br />'								  #   return a line break

		def paragraph(self, text, pre=None):			   
			return '<p>' + text + '</p>'				  #	  add paragraph tags
		
        
    However, let's say you want to add a specific style to your HTML document. In order
    to do this, you want to make each paragraph a class	of type "my_implementation". 
    Simply extend docx2Html and add what you need.
    
    Ex.
    
    class My_Implementation_of_Docx2Html(Docx2Html):
    
    	def paragraph(self, text, pre = None):
    		return <p class = "my_implementation"> + text + '</p>'
    		
	
	
	OR, let's say FOO is your new favorite markup language. Simply customize your own 
	new parser, overwritting the abstract methods of DocxParser
	
	Ex.
	
	class Docx2Foo(DocxParser):
	
		def linebreak(self):
			return '!!!!!!!!!!!!' #  because linebreaks in are denoted by '!!!!!!!!!!!!'
								  #  in the FOO markup langauge  :)
		

	
	
	
	

