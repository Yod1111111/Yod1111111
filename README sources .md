# Alternative Sources Retriever

**Multi-source research retrieval system accessing conventional and non-conventional knowledge repositories.**

Built for the Multi-Agent Deep Researcher hackathon to demonstrate retrieval beyond mainstream academic databases.

---

## What Makes This Different

Standard research assistants pull from: arXiv, Semantic Scholar, Google Scholar, Wikipedia.

This retriever adds:
- **Russian scientific papers** (Cyberleninka - bypassing language barriers)
- **Ancient digitized texts** (Sacred Texts - historical knowledge)
- **Plate tectonic reconstructions** (GPlates - geological deep time)
- **Real-time seismological data** (EMSC, IRIS - field observations)
- **Multilingual open access** (OpenAlex - 200M+ works, 40+ languages)

**The goal:** Surface knowledge that exists but is systematically excluded from mainstream retrieval systems due to language, age, format, or institutional gatekeeping.

---

## Sources Accessed (9 Total)

### Text & Paper Sources (5)

| Source | Type | Access | Coverage |
|--------|------|--------|----------|
| **OpenAlex** | API | Free, no key | 200M+ works, multilingual, open access |
| **arXiv** | API | Free, no key | Preprints before peer review |
| **Semantic Scholar** | API | Free key (optional) | Academic papers, citation network |
| **Cyberleninka** | Scraping | Open | Russian scientific papers |
| **Sacred Texts** | Scraping | Open | Ancient & historical texts digitized |

### Geological & Seismic Sources (4)

| Source | Type | Access | Coverage |
|--------|------|--------|----------|
| **GPlates** | API | Free, no key | Plate reconstructions 540Ma to present |
| **EMSC** | API | Free, no key | Mediterranean earthquakes real-time |
| **IRIS** | API | Free, no key | Global seismic network |
| **USGS** | API | Free, no key | US Geological Survey earthquake catalog |

---

## Installation

```bash
pip install -r requirements_retrievers.txt
```

**Dependencies:**
- requests
- beautifulsoup4
- arxiv
- lxml

---

## Usage

### Basic Text Search

```python
from retriever_unified import AlternativeSourceRetriever

retriever = AlternativeSourceRetriever()

# Search all text sources
results = retriever.search_all(
    query="plate tectonics Mediterranean",
    max_per_source=5,
    include_ancient=True,
    include_russian=True
)

# Convert to Pydantic Source format
sources = retriever.to_pydantic_sources(results)
```

### Tethys/Mediterranean Comprehensive Search

```python
# Combines text sources + geological data + seismicity
full_data = retriever.search_tethys_mediterranean(
    query="Tethys ocean closure seismic patterns",
    max_per_source=5
)

# Returns:
# - text_sources: papers, ancient texts, Russian research
# - plate_history: GPlates reconstructions
# - recent_seismicity: EMSC earthquake data
# - tethys_synthesis: full geological context
```

### Geological Data Only

```python
from retriever_geological import GeologicalDataRetriever

geo = GeologicalDataRetriever()

# Get Tethys closure sequence
tethys_data = geo.get_tethys_closure_data()

# Get Mediterranean earthquakes
context = geo.get_mediterranean_context(
    include_recent_earthquakes=True,
    include_plate_history=True,
    plate_times_ma=[0, 10, 50, 100, 250]
)
```

---

## Integration with Existing System

Replace your existing retriever with this:

```python
from retriever_unified import AlternativeSourceRetriever

class YourRetrieverAgent:
    def __init__(self):
        self.retriever = AlternativeSourceRetriever()
    
    def retrieve(self, query: str, max_results: int = 20):
        # Get alternative sources
        results = self.retriever.search_all(
            query=query,
            max_per_source=5,
            include_ancient=True,
            include_russian=True
        )
        
        # Remove duplicates
        results = self.retriever.filter_duplicates(results)
        
        # Convert to your Pydantic Source schema
        sources = self.retriever.to_pydantic_sources(results)
        
        return sources[:max_results]
```

---

## Demo Pre-Caching (Recommended)

For reliable demo videos, pre-cache your query results:

```python
from retriever_unified import AlternativeSourceRetriever
import json

retriever = AlternativeSourceRetriever()

# Run query once
demo_query = "Tethys ocean closure Mediterranean seismic"
full_data = retriever.search_tethys_mediterranean(demo_query, max_per_source=5)

# Save to file
with open("demo_sources_cached.json", "w") as f:
    json.dump(full_data, f, indent=2)
```

Then in your demo, load from cache instead of live API calls. This prevents failures if a site is down during recording.

---

## File Structure

```
├── retriever_apis.py           # OpenAlex, arXiv, Semantic Scholar, USGS
├── retriever_scrapers.py       # Cyberleninka, Sacred Texts
├── retriever_geological.py     # GPlates, EMSC, IRIS
├── retriever_unified.py        # Combines all sources
├── requirements_retrievers.txt # Dependencies
├── INTEGRATION_GUIDE.md        # Detailed integration steps
└── README.md                   # This file
```

---

## Testing

```bash
# Test all sources
python retriever_unified.py

# Test geological sources only
python retriever_geological.py

# Test individual components
python retriever_apis.py
python retriever_scrapers.py
```

---

## Performance Notes

**Fast sources (< 1 second):**
- OpenAlex, arXiv, Semantic Scholar, GPlates, EMSC, IRIS, USGS

**Slow sources (scraping, 2-5 seconds):**
- Cyberleninka, Sacred Texts

**For production:** Use async parallel retrieval or pre-cache common queries.

**For demo:** Pre-cache to ensure fast, reliable playback.

---

## Limitations & Future Work

**Current limitations:**
- Cyberleninka scraping may break if site HTML changes
- Sacred Texts returns section indexes, not full text search
- OneGeology WMS integration simplified (metadata only)
- No cross-session caching (every query hits APIs fresh)

**Future enhancements:**
- Add Chinese CNKI, Japanese J-STAGE
- Full WMS integration for OneGeology geological maps
- African Journals Online (AJOL)
- Indigenous knowledge repositories (TKDL, AIATSIS)
- Persistent cache with Redis
- Async parallel retrieval

---

## License

MIT License - use freely for research and education.

---

## Contact

Built for Multi-Agent Deep Researcher Hackathon  
Team: [Your Team Name]  
Date: May 2026

---

## Acknowledgments

Data sources:
- OpenAlex - openalex.org
- arXiv - arxiv.org  
- Semantic Scholar - semanticscholar.org
- Cyberleninka - cyberleninka.ru
- Sacred Texts - sacred-texts.com
- GPlates - gplates.org / earthbyte.org
- EMSC - seismicportal.eu
- IRIS - iris.edu
- USGS - usgs.gov
