##
## Bulkrays Copyright (C) 2012-2020 Jean-Daniel Pauget
## ... light http server C++ library ...
##
## jdbulkrayed@disjunkt.com  -  http://bulkrays.disjunkt.com/
##
## This file is part of Bulkrays.
##
## Loopsoids is free software; you can redistribute it and/or modify
## it under the terms of the GNU General Public License as published by
## the Free Software Foundation; either version 2 of the License, or
## (at your option) any later version.
##
## Loopsoids is distributed in the hope that it will be useful,
## but WITHOUT ANY WARRANTY; without even the implied warranty of
## MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
## GNU General Public License for more details.
##
## You should have received a copy of the GNU General Public License
## along with Loopsoids; if not, write to the Free Software
## Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
##
## you can also try the web at http://www.gnu.org/
##



ACLOCAL_AMFLAGS = -I m4

vimtest: simplefmap
	## ddd -args ./bulkrays --bind=127.0.0.1:10080 --user=$$USER --access_log=access_log --earlylog --console --p tagazou zonzon=2 p=3 BulkRays::ownsetoftests &
	## exit 0
	## ./hcpp2cpp testbb.hcpp -o testbb.cpp
	./simplefmap --bind=0.0.0.0:10080 --user=$$USER		\
	    --access_log=access_log --earlylog --console	\
	    --p tagazou zonzon=2 p=3				\
	    BulkRays::ownsetoftests				\
	    2>&1 | unbuffer -p tr ':' '='
##	    --debugparsereq \


noinst_PROGRAMS = simplefmap

simplefmap_SOURCES = testsite.cpp bootstrap.cpp simplefmap.cc
simplefmap_CXXFLAGS = @LOCALINCPATH@ @LIBMHASH_CFLAGS@
simplefmap_LDADD = @QICONNLDLIB@ @LIBMHASH_LDFLAGS@ @BULKRAYSLDLIB@
simplefmap_LDFLAGS = @QICONNLDFLAGS@ @BULKRAYSLDFLAGS@

		   
%.cc: %.hcpp
	hcpp2cpp $< -o $@


CLEANFILES = simplefmap.cc access_log

clean-local:
	:

distclean-local:
	rm m4/*.m4


