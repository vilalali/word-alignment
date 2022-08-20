		================= Word alignment using GIZA++ ===================
		################## INSTALLATION GUIDE##########
		1=> $ git clone https://github.com/vilalali/word-alignment.git
		2=> $ cd word-alignment
			$ ls -ltr
				GIZA++-v2
				mkcls-v2
		3=> Run the following Command to Install GIZA++-v2 and mkcls-v2:
		a=> Installation of GIZA++-v2:
		$ cd GIZA++-v2
		$ make clean
		$ make
		b=> Inastallation of mkcls-v2:
		$ cd mkcls-v2
		$ make clean
		$ make

		################ Word Alignment Guide ################
		1=> Create Source file:
			$ vi en1.txt
			Coppy the following text:
				India is a country of ancient culture and civilization
				Narendra Modi is the prime minister of India
				India is member of United Nations
		2=> Create the Targated File 
			$ vi hi1.txt
			Copy the following text:
				भारत प्राचीन संस्कृति और सभ्यता का देश है
				नरेंद्र मोदी भारत के प्रधान मंत्री हैं
				नरेंद्र मोदी भारत के प्रधान मंत्री हैं  
		3=> Now Run the following Command:
			$ ./GIZA++-v2/plain2snt.out [source_language_corpus] [target_language_corpus]
			$ ./GIZA++-v2/plain2snt.out en1.txt hi1.txt

		4=> Output Generated FIles:
			It will generate 4 files:
			a) hi1.vcb, hi1_en1.snt, en1.vcb, en1_hi1.snt
			b) en1.vcb will represent the following data:
			c) en1.vcb: this file contains the eng word its id and frequency. format is [ID word frequency] 

		a) The output of en1.vcb contains the french words its id and frequency. format is [ID word frequency] 
		2 India 3
		3 is 3
		4 a 1
		5 country 1
		6 of 3
		7 ancient 1
		8 culture 1
		9 and 1
		10 civilization 1
		11 Narendra 1
		12 Modi 1
		13 the 1
		14 prime 1
		15 minister 1
		16 member 1
		17 United 1
		18 Nations 1

		d) Similarly the hi1.vcb contains the french words its id and frequency. format is [ID word frequency] 
		2 भारत 3
		3 प्राचीन 1
		4 संस्कृति 1
		5 और 1
		6 सभ्यता 1
		7 का 1
		8 देश 1
		9 है 1
		10 नरेंद्र 2
		11 मोदी 2
		12 के 2
		13 प्रधान 2
		14 मंत्री 2
		15 हैं 2

		e) en1_hi1.snt : It contains the parallel sentences (source sentence Ids according to the alignment with target sentence Ids
		1
		2 3 4 5 6 7 8 9 10 
		2 3 4 5 6 7 8 9 
		1
		11 12 3 13 14 15 6 2 
		10 11 2 12 13 14 15 
		1
		2 3 16 6 17 18 
		10 11 2 12 13 14 15 

		f) hi1_en1.snt : It contains the parallel sentences (target sentence Ids according to the alignment with source sentence Ids
		1
		2 3 4 5 6 7 8 9 
		2 3 4 5 6 7 8 9 10 
		1
		10 11 2 12 13 14 15 
		11 12 3 13 14 15 6 2 
		1
		10 11 2 12 13 14 15 
		2 3 16 6 17 18 

		5=> Type following commands for Making class and cooccurrence:
			$ ./mkcls-v2/mkcls -p[source_language_corpus] -V[source_language_corpus].vcb.classes
			$ ./mkcls-v2/mkcls -p[target_language_corpus] -V[target_language_corpus].vcb.classes

		a) Example:
			$./mkcls-v2/mkcls -pen1.txt -Ven1.txt.vcb.classes
			$./mkcls-v2/mkcls -phi1.txt -Vhi1.txt.vcb.classes
		b) These commands will generate following four files:
			en1.txt.vcb.classes.cats, en1.txt.vcb.classes, hi1.txt.vcb.classes.cats, hi1.txt.vcb.classes
		c) '*.vcb.classes' file contains the class category Id associated with each of the tokens which is taken from the *.vcb.classes.cat file.

		6=> create output directory using command 
			$mkdir myout

		7=> Now use GIZA++ to build your dictionary: (-S is the source language, -T is the target language, -C is the generated aligned text file, and -o is the output file prefix): 
			./GIZA++ -S [source_language_corpus].vcb -T [target_language_corpus].vcb -C [target_language_corpus]_[source_language_corpus].snt -o [prefix] -outputpath [output_folder]
		Example. : 
			$ ./GIZA++ -S hi1.vcb -T en1.vcb -C hi1_en1.snt -outputpath myout -o test
			If you see ERROR: NO COOCURRENCE FILE GIVEN! when running GIZA++, you may need to change Makefile to compile GIZA++

			Before:

			CFLAGS_OPT = $(CFLAGS) -O3 -funroll-loops -DNDEBUG -DWORDINDEX_WITH_4_BYTE 
				-DBINARY_SEARCH_FOR_TTABLE -DWORDINDEX_WITH_4_BYTE

			After:
			CFLAGS_OPT = $(CFLAGS) -O3 -funroll-loops -DNDEBUG -DWORDINDEX_WITH_4_BYTE

		8=> It will generate the output files in myout/ directory and out of the variuos files file with name [prefix].actual.ti.final. In our case the final output file will be in output dir the wit the name: 
		
		test.actual.ti.final
		It contains the alignment of source and target words according to their probability value:
		FINAL Output Will Be:
		
		test.actual.ti.final:
		
		मंत्री Nations 0.5
		हैं United 0.5
		नरेंद्र member 0.5
		मंत्री minister 0.5
		सभ्यता civilization 1
		और culture 0.995885
		हैं prime 0.5
		प्राचीन NULL 8.25337e-06
		नरेंद्र Narendra 0.5
		भारत NULL 0.00130641
		भारत is 0.0465133
		मोदी Modi 0.264116
		भारत of 0.652697
		भारत India 0.081918
		के NULL 0.358273
		संस्कृति NULL 3.06593e-05
		के India 0.641727
		प्रधान NULL 1
		देश civilization 1
		संस्कृति ancient 0.999956
		है and 1
		और ancient 0.00411528
		मोदी is 0.735884
		का civilization 1
		भारत the 0.217566
		प्राचीन country 0.999992
		संस्कृति country 1.38068e-05
