# eDNA_libraries
cp -r 8209-P2-2tol/* # to copy all the folders in a folder
GJ's pipeline
conda install cutadapt
module load cutadapt/4.1-gimkl-2022a-Python-3.10.5
module load eDNA #loads everything below, must load everytime in new windows as well
Currently Loaded Modules:
  1) XALT/minimal                                   38) Tcl/8.6.10-GCCcore-9.2.0
  2) NeSI                                      (S)  39) Tk/8.6.10-GCCcore-9.2.0
  3) slurm                                          40) Python/3.8.2-gimkl-2020a
  4) libpmi/2-slurm                                 41) PCRE2/10.35-GCCcore-9.2.0
  5) numactl/2.0.14-GCC-11.3.0                      42) NLopt/2.6.1-GCCcore-9.2.0
  6) UCX/1.12.1-GCC-11.3.0                          43) GMP/6.2.0-GCCcore-9.2.0
  7) AlwaysIntelMKL/1.0                             44) Java/11.0.4
  8) imkl-FFTW/2022.0.2-gimpi-2022a                 45) nodejs/14.16.1-GCCcore-9.2.0
  9) git/2.23.3                                     46) GCCcore/9.2.0
 10) ZeroMQ/4.3.4-GCCcore-11.3.0                    47) zlib/1.2.11-GCCcore-9.2.0
 11) JupyterLab/.2023.1.0-gimkl-2022a-3.5.3    (H)  48) OpenSSL/1.1.1k-GCCcore-9.2.0
 12) craype-broadwell                               49) ICU/50.2-GCCcore-9.2.0
 13) craype-network-infiniband                      50) R/4.1.0-gimkl-2020a
 14) binutils/2.32-GCCcore-9.2.0                    51) expat/2.2.9-GCCcore-9.2.0
 15) GCC/9.2.0                                      52) OpenJPEG/2.3.1-GCCcore-9.2.0
 16) impi/2019.6.166-GCC-9.2.0                      53) HDF/4.2.15-GCCcore-9.2.0
 17) imkl/2020.0.166-gimpi-2020a                    54) KEALib/1.4.12-gimpi-2020a
 18) gimkl/2020a                                    55) LibTIFF/4.0.10-GCCcore-9.2.0
 19) bzip2/1.0.8-GCCcore-9.2.0                      56) PROJ/6.3.1-GCCcore-9.2.0
 20) libpng/1.6.37-GCCcore-9.2.0                    57) libgeotiff/1.5.1-GCCcore-9.2.0
 21) freetype/2.10.1-GCCcore-9.2.0                  58) FreeXL/1.0.5-GCCcore-9.2.0
 22) libjpeg-turbo/2.0.2-GCCcore-9.2.0              59) GEOS/3.8.0-GCCcore-9.2.0
 23) ncurses/6.1-GCCcore-9.2.0                      60) libspatialite/4.3.0a-GCCcore-9.2.0
 24) XZ/5.2.4-GCCcore-9.2.0                         61) PostgreSQL/11.7-GCC-9.2.0
 25) libxml2/2.9.10-GCCcore-9.2.0                   62) GDAL/3.0.4-gimkl-2020a
 26) libxslt/1.1.34-GCCcore-9.2.0                   63) UDUNITS/2.2.26-GCCcore-9.2.0
 27) gimpi/2020a                                    64) R-Geo/4.1.0-gimkl-2020a
 28) Szip/2.1.1-GCCcore-9.2.0                       65) GSL/2.6-GCCcore-9.2.0
 29) HDF5/1.10.5-gimpi-2020a                        66) R-bundle-Bioconductor/3.13-gimkl-2020a-R-4.1.0
 30) cURL/7.64.0-GCCcore-9.2.0                      67) MUSCLE/3.8.1551
 31) PCRE/8.43-GCCcore-9.2.0                        68) NASM/2.14.02-GCCcore-9.2.0
 32) netCDF/4.7.3-gimpi-2020a                       69) ISA-L/2.30.0-gimkl-2020a
 33) libreadline/8.0-GCCcore-9.2.0                  70) cutadapt/3.5-gimkl-2020a-Python-3.8.2
 34) SQLite/3.31.1-GCCcore-9.2.0                    71) VSEARCH/2.17.1-GCC-9.2.0
 35) METIS/5.1.0-GCCcore-9.2.0                      72) MultiQC/1.9-gimkl-2020a-Python-3.8.2
 36) tbb/2019_U9-GCCcore-9.2.0                      73) FastQC/0.11.9
 37) SuiteSparse/5.6.0-gimkl-2020a-METIS-5.1.0      74) eDNA/2021.11-gimkl-2020a-Python-3.8.2

  Where:
   H:  Hidden Module

#NCBI
   crabs db_download -s ncbi -db nucleotide -q '12S[All Fields] AND txid7898[Organism:exp] AND mitochondrion[filter]' -o fish_12S_ncbi.fasta -e     
   ryan.r.easton@gmail.com
#in silico PCR analysis
   crabs insilico_pcr -i fish_12S_ncbi.fasta -o fish_12S_ncbi_insilico.fasta -f GTCGGTAAAACTCGTGCCAGC -r CATAGTGGGGTATCTAATCCCAGTTTG
#assign taxa
  crabs db_download --source taxonomy #use gunzip to unzip file
  crabs assign_tax -i fish_12S_ncbi_insilico.fasta -o fish_12S_ncbi_insilico_tax.tsv -a nucl_gb.accession2taxid -t nodes.dmp -n names.dmp
#dereplicate
  crabs dereplicate -i fish_12S_ncbi_insilico_tax.tsv -o fish_12S_ncbi_insilico_tax_derep.tsv -m uniq_species
  crabs seq_cleanup -i fish_12S_ncbi_insilico_tax_derep.tsv -o fish_12S_ncbi_insilico_tax_derep_clean.tsv -e yes -s yes -na 0
#export reference database
  crabs tax_format -i fish_12S_ncbi_insilico_tax_derep_clean.tsv -o fish_12S_ncbi_insilico_tax_derep_clean_sintax.fasta -f sintax
