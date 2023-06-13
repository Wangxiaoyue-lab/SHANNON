
# argparse



```python
import argparse  
parser = argparse.ArgumentParser(formatter_class=argparse.RawDescriptionHelpFormatter, description='Get script info')  
parser.add_argument('-i', '--input', required = True, help="Input file for marker annotation(Only CSV format supported).")
args = parser.parse_args()
```


函数式写法

```python
def get_argparse():
	parser = argparse.ArgumentParser(formatter_class=argparse.RawDescriptionHelpFormatter, description='Get script info')  
	parser.add_argument('-i', '--input', required = True, help="Input file for marker annotation(Only CSV format supported).")
	args = parser.parse_args()
	return args
```


面向对象式写法

```python
class get_argparse(object):
	def __init__(self):
		pass
	def get_args(self):
		parser = argparse.ArgumentParser(formatter_class=argparse.RawDescriptionHelpFormatter, description='Get script info')  
		parser.add_argument('-i', '--input', required = True, help="Input file for marker annotation(Only CSV format supported).")
		return parser


G_args = get_argparse()
G_parser = G_args.get_args()
args = G_parser.parse_args()

```


# click
