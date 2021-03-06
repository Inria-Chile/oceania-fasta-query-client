# OcéanIA Query Fasta

OcéanIA Query Fasta lets you query large FASTA files available in the [OcéanIA Platform](https://oceania.inria.cl/) for extracting parts of biologic sequences 

## Create and activate a Python virtual environment

```sh
git clone https://github.com/Inria-Chile/oceania-query-fasta.git
cd oceania-query-fasta
python3 -m venv venv
. venv/bin/activate
pip install -r requirements.txt
```

## Installation from PyPi

FASTA format is a text-based format for representing either nucleotide sequences or amino acid sequences, in which base pairs or amino acids are represented using single-letter codes. A sequence in FASTA format begins with a single-line description, followed by lines of sequence data. The description line is distinguished from the sequence data by a greater-than (">") symbol in the first column. An example sequence in FASTA format is:

```fasta
>TARA_X000000368_G_scaffold34_1_gene23 strand:- start:199 stop:642 length:444 start_codon:yes stop_codon:yes gene_type:complete
ATGAATACTCTTACTCGAATAAGCCTGACAATTTTTTTACTTTTGATGGCAGCTGTCTAT
TTACCAATTGGTTTATGGGCAATTATTGCTCCAGCTCAGGATGCCCTTGGTTTAGAACTA
CCTTCTTTTTATGAAGCTGTAGGCTTATCTGTAATATCTCCAATTGGGTATTCAGAATTT
GCAGGTATATATGGGGGCATTAATATTGTCATTGGCGTGATGTTCCTAATAGGCGTTTTT
AAAAAACAGGTCGGACTATTTGCTATAAAAGTTCTTGTATTTCTTGTTGGCTCAATAGCT
CTTGGAAGATTCTTGCTAATGTTGCTTGGATCCCAGGCAGGATTACCTGCAGAAATTAAT
GCTTTTCTTATCTTTGAAATAATTGTTTTCTTTATAGGTATTATTTTTATTAAAGTCCTA
AAAAACACTGATCATGTTACTTAG
```

## Installation and usage
### Installation

OcéanIA Query Fasta can be installed by running `pip install oceania-query-fasta`. It requires Python >= 3.6 to run.

### Usage

The library can be used as a command line tool or imported as a python library.

### As a python package

```python
from oceania import get_sequences_from_fasta

TARA_SAMPLE_ID = "TARA_A100000171"

# REQUEST_PARAMS is a list of tuples that identify subsequences to extract
# each tuple must have the values (sequence_id, start_index, stop_index, sequence_type)
# sequence type accepted values are [raw, complement, reverse_complement], optional value if ommited defaults to "raw".
REQUEST_PARAMS = [
            ("TARA_A100000171_G_scaffold48_1", 10, 50, "complement"),
            ("TARA_A100000171_G_scaffold48_1", 10, 50),
            ("TARA_A100000171_G_scaffold48_1", 10, 50, "reverse_complement"),
            ("TARA_A100000171_G_scaffold181_1", 0, 50),
            ("TARA_A100000171_G_scaffold181_1", 100, 200),
            ("TARA_A100000171_G_scaffold181_1", 200, 230),
            ("TARA_A100000171_G_scaffold493_2", 54, 76),
            ("TARA_A100000171_G_scaffold50396_2", 87, 105),
            ("TARA_A100000171_G_C2001995_1", 20, 635),
            ("TARA_A100000171_G_C2026460_1", 0, 100),
        ]

request_result = get_sequences_from_fasta(
    TARA_SAMPLE_ID,
    REQUEST_PARAMS
)

# get_sequences_from_fasta returns a pandas.DataFrame with the extracted sequences
print(request_result)
```

### Command line

In the command line the query feature is available as `oceania query-fasta <key> <query_file> <output_format> <output_file>`

```bash
> oceania query-fasta -h
Usage: oceania query-fasta [OPTIONS] <key> <query_file> <output_format> <output_file>

  Extract secuences from a fasta file in the OcéanIA Platform.

  <sample_id> sample id in the OcéanIA Platform
  <query_file> CSV file containing the values to query.
               Each line represents a sequence to extract in the format "sequence_id,start,end,type"
               "sequence_id" sequence ID
               "start" start index position of the sequence to be extracted
               "end" end index position of the sequence to extract
               "type" type of the sequence to extract
                      options are ["raw", "complement", "reverse_complement"]
                      type value is optional, if not provided default is "raw"
  <output_format> results format
                  options are ["csv", "fasta"]
  <output_file> name of the file to write the results
```

#### Example

```bash
> oceania query-fasta data/raw/tara/OM-RGC_v2/assemblies/TARA_A100000171.scaftig.gz query.csv csv example.output.csv
```

query.csv:
```csv
TARA_A100000171_G_scaffold48_1,10,50,complement
TARA_A100000171_G_scaffold48_1,10,50,raw
TARA_A100000171_G_scaffold48_1,10,50,reverse_complement
TARA_A100000171_G_scaffold181_1,0,50
TARA_A100000171_G_scaffold181_1,100,200
TARA_A100000171_G_scaffold181_1,200,230
```

### As a python package

```python
from oceania import OceaniaClient

# Create a client instance
oceania_client = OceaniaClient()

# Execute a query
oceania_client.get_secuences_from_fasta_to_file(key, positions, output_format, output_file)
```

#### Example

```python
from oceania import OceaniaClient

oceania_client = OceaniaClient()

key = "data/raw/tara/OM-RGC_v2/assemblies/TARA_A100000171.scaftig.gz"
positions = [
    ["TARA_A100000171_G_scaffold48_1", 10, 50, "complement"],
    ["TARA_A100000171_G_scaffold48_1", 10, 50],
    ["TARA_A100000171_G_scaffold48_1", 10, 50, "reverse_complement"],
    ["TARA_A100000171_G_scaffold181_1", 0, 50],
    ["TARA_A100000171_G_scaffold181_1", 100, 200],
    ["TARA_A100000171_G_scaffold181_1", 200, 230],
]

oceania_client.get_secuences_from_fasta_to_file(
    key,
    positions,
    "csv",
    "test_output.csv"
)
```

# Installation from the local repo

Perform the following steps in order to install this program:

```sh
git clone https://gitlab.com/Inria-Chile/oceania-query-fasta.git
cd oceania-query-fasta
pip3 uninstall oceania-query-fasta
# Version of local
pip install -e .
# Version of PyPi interal
pip3 install -i http://pypi.inriadev.cl:6543/simple/ oceania-query-fasta
# Version of Test PyPi
pip3 install -i https://test.pypi.org/simple/ oceania-query-fasta
pip3 install oceania-query-fasta
```

# Updates in PyPi

Perform the following steps in order to publish new versions to PyPi:

```sh
sudo rm -rf build oceania-query-fasta.egg-info dist
python3 setup.py sdist bdist_wheel
twine check dist/*
# Version of PyPi interal
twine upload --verbose --repository-url http://pypi.inriadev.cl:6543 dist/*
# Version of Test PyPi
twine upload --verbose --repository-url https://test.pypi.org/legacy/ dist/*
# Version of PyPi
twine upload --verbose dist/*
```