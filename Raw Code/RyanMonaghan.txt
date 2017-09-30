#!/bin/bash

#OS Bash Shell Programming Assignment 2
#Name: Ryan Monaghan
#ID: R00115129
#Class: IT2-A

if ((${#}<2))
	then
		echo "${0}:ERROR: No arguments given." 1>&2 
		echo "${0}:USAGE: <existing directory> followed by <destination directory>." 1>&2
		exit 1 
fi

if test ! -d "${1}" 
	then
		echo "${0}:ERROR: Source directory '${1}' does not exist!" 1>&2
		echo "${0}:USAGE: Enter a correct directory name." 1>&2
		exit 2
fi

if test -d "${2}" 
	then
		echo "${0}:ERROR: Destination directory '${2}' already exists!" !>&2
		echo "${0}:USAGE: Enter a unique destination name." 1<&2
		exit 3
fi

if test -e "${2}" 
	then
		echo "${0}:ERROR: '${2}' matches the name of an existing file!" 1>&2
		echo "${0}:ERROR: Enter a unique name." 1>&2
		exit 4
fi

mkdir ${2} 

if test ! -d "${2}" 
	then
		echo " ${0}:ERROR: Directory '${2}' has not been created." 
		echo "${0}:USAGE: Please try again." 1>&2 
		exit 5
fi

typeset -i cpFile 
typeset -i fcpFile 

for file in $(ls "${1}") 
	do
		if cp "${1}/${file}" "${2}" 
	then
		((cpFile++)) 
	else
		echo "${0}:ERROR: Copy operation failed for 1 or more files!" 1<&2 
		((fcpFile++)) 
		fi
done 

if test ${fcpFile} == 0 
	then
		echo "${0}:USAGE:  All "${cpFile}" files have been copied successfully!" 1>&2
		exit 6
fi

if test ${fcpFile} != 0 
	then
		echo "${0}:ERROR: "${fcpFile}" failed to be copied into "${2}"." 1>&2  
		echo "${0}:WARNING: "${cpFile}" files were successfully copied, while "${fcpFile}" failed to be copied into "${2}"." 
		exit 7
fi

if test ${cpFile} == 0 
	then
		echo "${0}:WARNING: NONE of the "${cpFile}" files were successfully copied." 1>&2 
		exit 8
fi