Title: Ramblings...
Date: 2010-12-03 10:20
Category: Python
Tags: pelican, publishing
Author: Alexis Metaireau
Summary: STickled pink on black eyed wafers. Middle management cut shady deals

.
	Tickled pink on black eyed wafers
	Calling home to familiar faces
	Broken tongues of cajun spoken
	Goverment collusion BP sponsored

	Dolphin stillbornes
	Poison seafood
	Broken system criminals rampant
	Middle management cut shady deals
	Led 
    
~

This appeared to work pretty well on my tests, but continued to fail on 
Facebook puzzle submission for some reason I couldn't track down at the 
time. In my frustration, I went to the internet. Turns out, this problem 
is actually known as [list maximal 
cliques](http://en.wikipedia.org/wiki/Clique_problem#Listing_all_maximal 
_cliques), a category of clique problem. The prior link has more 
information about cliques, which are subgraphs that have the friend 
property desired by this problem. Additionally, there is a known "pretty 
good" strategy for solving this problem, the [Bron Kerbosch 
algorithm](http://en.wikipedia.org/wiki/Bron%E2%80%93Kerbosch_algorithm) 
. 

I went all out and applied a couple optimizations, namely (warning: 
abstract pdfs) 
[pivoting](ftp://ftp-sop.inria.fr/geometrica/fcazals/papers/ncliques.pdf) and
[degeneracy ordering](http://drops.dagstuhl.de/opus/volltexte/2011/2935/pdf/10441.EppsteinDavid.Paper.2935.pdf),
which are two techniques used to cut down on the number of recursive calls.
At this point my code looked something like

    def generate_degeneracy_ordering
        d = []  #degree buckets
        dw = {} #degree for each vertex
        $sparse.each_pair do |vertex, neighbors|
            deg = neighbors.size
            d[deg] ||= []
            d[deg].push vertex
            dw[vertex] = deg
        end
        d.each_index {|i| d[i] ||= []}
        $sparse.size.times do
            vertex = d.find {|x| !x.empty?}.pop
            $degen.push vertex
            for neighbor in $sparse[vertex]
                if d[dw[neighbor]].delete neighbor
                    dw[neighbor] -= 1
                    d[dw[neighbor]].push neighbor
                end
            end
        end
    end

    def bron_kerbosch(set, points, exclude, pivot_neighbors=nil)
        if points.empty?
            $sets.push set.sort.join(', ') if set.size > 2 and exclude.empty?
            return
        end

        pivot_neighbors ||= (exclude.empty? or $sparse[points.last].size > $sparse[exclude.last].size) ?
            $sparse[points.last] : $sparse[exclude.last]
            
        points.each_with_index do |vertex, i|
            next if pivot_neighbors.include? vertex
            points[i] = nil
            bron_kerbosch(set + [vertex],
                          points & $sparse[vertex],
                          exclude & $sparse[vertex])
            exclude.push vertex
        end
    end

    exit unless ARGV.size == 1
    ingest(ARGV[0])

    generate_degeneracy_ordering
    before = []
    after = $degen[1..$degen.size-1]
    $degen.each do |vertex|
        intersect = after & $sparse[vertex]
        bron_kerbosch([vertex],
                      intersect,
                      before & $sparse[vertex],
                      $sparse[intersect.last]) #last elements in $degen have highest degrees
        before.push vertex
        after.shift
    end

Which let me parse a 120MB input file in 1 minute flat. However, for 
some reason this was slower than my naive solution, which could do it, 
ironically, in 48s. It did pass the Facebook puzzle checker, though, so 
at least I had that. If anyone has any insight into why my naive 
solution appears to outperform bron_kerbosch I would greatly appreciate 
it, as it does not seem correct. My guess is slow implementations in 
Ruby of Array "set-like" operators (&, +).

### final thoughts

I spent 2-3 days stuck on bugs that amounted to 

* parsing input off by one whitespace
* output missing a final newline
* mysterious bug that turned out to be RUBY 1.8.6 NOT HAVING MAX_BY AND
ME TESTING IN 1.8.7

Facebook absolutely does not provide you any clues as to what went wrong 
besides there being a syntax error in your build. In this case it falls 
on you to both duplicate their runtime environment and generate workable 
tests. But finally seeing the confirmation email is definitely worth it. 
Here are the results after finally getting my naive solution running. 

>Dear submitter,
>
>Thank you for your submission of a puzzle solution to Facebook! After running your solution to peaktraffic (received on February 28, 2011, 4:07 pm), I have determined it to be correct. Your solution ran for 1162.186 ms on its longest test case.

Here are my two [solutions](https://github.com/vincentwoo/rubycode/blob/master/peaktraffic)
to this problem. The codepath that executes the Bron Kerbosch algorithm is not active but
fairly obvious. They both work.

