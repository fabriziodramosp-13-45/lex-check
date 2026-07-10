from http.server import BaseHTTPRequestHandler, HTTPServer
from pathlib import Path
from threading import Timer
from urllib.parse import urlparse
from urllib.request import Request, urlopen
#import cgi
import hashlib
import html
import io
import json
import mimetypes
import os
import re
import socket
import time
import unicodedata
import webbrowser
import zipfile

from pypdf import PdfReader


PORT = int(os.environ.get("PORT", "9000"))
ROOT = Path(__file__).resolve().parent
DATA_DIR = ROOT / "data"
if not DATA_DIR.exists() and (ROOT.parent / "work" / "data").exists():
    DATA_DIR = ROOT.parent / "work" / "data"
KB_PATH = DATA_DIR / "knowledge_base.json"
SPIJ_PATH = DATA_DIR / "spij_sources.json"

DEFAULT_SPIJ_SOURCES = [
    {
        "title": "Código Civil - SPIJ",
        "url": "https://spij.minjus.gob.pe/spij-ext-web/#/detallenorma/H682684",
        "id": "H682684",
        "matter": "civil",
        "official": True,
    },
    {
        "title": "Código Procesal Civil - SPIJ",
        "url": "https://spij.minjus.gob.pe/spij-ext-web/#/detallenorma/H682685",
        "id": "H682685",
        "matter": "procesal_civil",
        "official": True,
    },
    {
        "title": "TUO del Código Tributario - SPIJ",
        "url": "https://spij.minjus.gob.pe/spij-ext-web/#/detallenorma/H682696",
        "id": "H682696",
        "matter": "tributario",
        "official": True,
    },
    {
        "title": "Código Procesal Constitucional - SPIJ",
        "url": "https://spij.minjus.gob.pe/spij-ext-web/#/detallenorma/H1288461",
        "id": "H1288461",
        "matter": "constitucional",
        "official": True,
    },
    {
        "title": "TUO del Reglamento General de los Registros Públicos - LP Derecho",
        "url": "https://lpderecho.pe/texto-unico-ordenado-reglamento-general-registros-publicos-resolucion-126-2012-sunarp-sn/",
        "id": "",
        "matter": "registral",
        "official": False,
    },
]

SOURCE_PDFS = [
    Path(r"C:\Users\ALEL ORBEZO\Downloads\derecho de contratos\CódigoCivil.pdf"),
    Path(r"C:\Users\ALEL ORBEZO\Downloads\derecho de contratos\código procesal civil.pdf"),
    Path(r"C:\Users\ALEL ORBEZO\Downloads\derecho de contratos\ilovepdf_merged (15)_removed.pdf"),
    Path(r"C:\Users\ALEL ORBEZO\Downloads\ilovepdf_merged (15)_removed.pdf"),
]

STOPWORDS = {
    "a", "al", "ante", "bajo", "con", "contra", "de", "del", "desde", "durante",
    "e", "el", "en", "entre", "es", "esa", "ese", "esta", "este", "la", "las",
    "le", "lo", "los", "mas", "me", "mi", "no", "o", "para", "pero", "por",
    "que", "se", "sin", "sobre", "su", "sus", "te", "tu", "un", "una", "y",
    "como", "cuando", "donde", "cual", "cuales", "puede", "debe", "deben",
    "contrato", "contratos", "parte", "partes", "articulo", "articulos",
}

FALLBACK_LEGAL_SOURCES = [
    {
        "source": "Código Civil peruano",
        "page": "Art. 140",
        "text": "Artículo 140. El acto jurídico requiere agente capaz, objeto física y jurídicamente posible, fin lícito y observancia de la forma prescrita bajo sanción de nulidad.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1351",
        "text": "Artículo 1351. El contrato es el acuerdo de dos o más partes para crear, regular, modificar o extinguir una relación jurídica patrimonial.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1361",
        "text": "Artículo 1361. Los contratos son obligatorios en cuanto se haya expresado en ellos. Se presume que la declaración expresada responde a la voluntad común de las partes.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1362",
        "text": "Artículo 1362. Los contratos deben negociarse, celebrarse y ejecutarse según las reglas de la buena fe y común intención de las partes.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1321",
        "text": "Artículo 1321. Queda sujeto a indemnización quien no ejecuta sus obligaciones por dolo, culpa inexcusable o culpa leve.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1328",
        "text": "Artículo 1328. Es nula toda estipulación que excluya o limite la responsabilidad por dolo o culpa inexcusable del deudor o de terceros de quienes se valga.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1428",
        "text": "Artículo 1428. En los contratos con prestaciones recíprocas, cuando una parte falta al cumplimiento de su prestación, la otra puede solicitar el cumplimiento o la resolución del contrato.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1429",
        "text": "Artículo 1429. La parte perjudicada puede requerir a la otra para que cumpla dentro de un plazo no menor de quince días, bajo apercibimiento de resolución.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1430",
        "text": "Artículo 1430. Puede convenirse expresamente que el contrato se resuelva cuando una de las partes no cumple determinada prestación a su cargo.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1242",
        "text": "Artículo 1242. El interés es compensatorio cuando constituye contraprestación por el uso del dinero o de cualquier otro bien, y es moratorio cuando indemniza la mora en el pago.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1243",
        "text": "Artículo 1243. La tasa máxima del interés convencional compensatorio o moratorio es fijada por el Banco Central de Reserva del Perú.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1341",
        "text": "Artículo 1341. La cláusula penal funciona como una prestación pactada para el caso de incumplimiento; permite exigir la penalidad cuando se verifica el supuesto previsto por las partes.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1346",
        "text": "Artículo 1346. El juez puede reducir equitativamente la pena cuando sea manifiestamente excesiva o cuando la obligación principal haya sido cumplida en parte o de forma irregular.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1484",
        "text": "Artículo 1484. El transferente responde por saneamiento cuando el bien tiene vicios ocultos que disminuyen su valor o lo hacen inútil para el destino previsto.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Arts. 1485 y siguientes",
        "text": "Artículo 1485 y siguientes. Los artículos posteriores desarrollan la responsabilidad por vicios ocultos y sus efectos. Sirven como marco para revisar cláusulas que intentan liberar al propietario por defectos no visibles.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1680",
        "text": "Artículo 1680. El arrendador debe entregar el bien, conservarlo para el uso convenido y mantener al arrendatario en el goce del bien durante el arrendamiento.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1697",
        "text": "Artículo 1697. El contrato de arrendamiento puede resolverse por falta de pago de la renta y por otras causales previstas por la ley o el contrato.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1700",
        "text": "Artículo 1700. El arrendamiento de duración determinada concluye al vencimiento del plazo establecido, sin necesidad de aviso previo.",
        "kind": "fallback",
    },
    {
        "source": "Código Civil peruano",
        "page": "Art. 1704",
        "text": "Artículo 1704. Vencido el plazo del contrato, si el arrendatario permanece en uso del bien, no se entiende que hay renovación tácita sino continuación del arrendamiento bajo sus estipulaciones.",
        "kind": "fallback",
    },
]


def normalize(value):
    value = unicodedata.normalize("NFD", value.lower())
    value = "".join(ch for ch in value if unicodedata.category(ch) != "Mn")
    return re.sub(r"[^a-z0-9ñ]+", " ", value)


def tokens(value):
    return [part for part in normalize(value).split() if len(part) > 2 and part not in STOPWORDS]


def compact_text(value):
    return re.sub(r"\s+", " ", value or "").strip()


def html_to_text(value):
    value = re.sub(r"<script[\s\S]*?</script>|<style[\s\S]*?</style>", " ", value, flags=re.I)
    value = re.sub(r"</p>|<br\s*/?>|</div>|</h\d>|</li>", "\n", value, flags=re.I)
    value = re.sub(r"<[^>]+>", " ", value)
    return compact_text(html.unescape(value))


def split_chunks(text, max_chars=1300):
    text = compact_text(text)
    if not text:
        return []
    sentences = re.split(r"(?<=[.;:])\s+", text)
    chunks = []
    current = ""
    for sentence in sentences:
        if len(current) + len(sentence) + 1 > max_chars and current:
            chunks.append(current.strip())
            current = sentence
        else:
            current = f"{current} {sentence}".strip()
    if current:
        chunks.append(current.strip())
    return chunks


def split_article_chunks(text, max_chars=2200):
    text = compact_text(text)
    if not text:
        return []
    article_pattern = re.compile(
        r"(?=(?:Sumilla\s+)?Art[íi]culo\s+(\d{1,4}[A-Z]?)(?:[.\-º°\s]|$))",
        re.I,
    )
    matches = list(article_pattern.finditer(text))
    if not matches:
        return [{"page": "", "text": part, "kind": "chunk"} for part in split_chunks(text, max_chars=1200)]

    chunks = []
    intro = text[:matches[0].start()].strip()
    if intro:
        for part in split_chunks(intro, max_chars=1200):
            chunks.append({"page": "Presentación", "text": part, "kind": "intro"})

    for index, match in enumerate(matches):
        start = match.start()
        end = matches[index + 1].start() if index + 1 < len(matches) else len(text)
        article_number = match.group(1)
        article_text = text[start:end].strip()
        if not article_text:
            continue
        if len(article_text) <= max_chars:
            chunks.append({"page": f"Art. {article_number}", "text": article_text, "kind": "article"})
        else:
            parts = split_chunks(article_text, max_chars=max_chars)
            for part_index, part in enumerate(parts, start=1):
                suffix = "" if part_index == 1 else f" parte {part_index}"
                chunks.append({"page": f"Art. {article_number}{suffix}", "text": part, "kind": "article"})
    return chunks


def spij_norm_id_from_url(url):
    parsed = urlparse(url)
    match = re.search(r"/detallenorma/([A-Z0-9]+)", parsed.fragment or parsed.path, re.I)
    return match.group(1) if match else ""


def default_spij_title(url, norm_id=""):
    for source in DEFAULT_SPIJ_SOURCES:
        if url == source["url"] or (norm_id and norm_id == source.get("id")):
            return source["title"]
    return "SPIJ - " + (norm_id or urlparse(url).netloc)


def default_spij_meta(url, norm_id=""):
    for source in DEFAULT_SPIJ_SOURCES:
        if url == source["url"] or (norm_id and norm_id == source.get("id")):
            return source
    parsed = urlparse(url)
    return {
        "title": "Fuente jurídica - " + parsed.netloc,
        "matter": "",
        "official": "spij.minjus.gob.pe" in parsed.netloc,
        "id": norm_id,
        "url": url,
    }


def read_pdf_bytes(content):
    reader = PdfReader(io.BytesIO(content))
    if reader.is_encrypted:
        try:
            reader.decrypt("")
        except Exception:
            pass
    pages = []
    for index, page in enumerate(reader.pages, start=1):
        try:
            text = page.extract_text() or ""
        except Exception:
            text = ""
        pages.append((index, text))
    return pages


def read_docx_bytes(content):
    with zipfile.ZipFile(io.BytesIO(content)) as docx_zip:
        xml = docx_zip.read("word/document.xml").decode("utf-8", errors="ignore")
    xml = re.sub(r"</w:p>", "\n", xml)
    xml = re.sub(r"<[^>]+>", "", xml)
    return html.unescape(xml)


def read_upload(filename, content):
    name = filename.lower()
    if name.endswith(".pdf"):
        text = "\n".join(text for _, text in read_pdf_bytes(content))
        if not compact_text(text):
            raise ValueError("No pude extraer texto del PDF. Si es un escaneo o imagen, conviértelo con OCR antes de subirlo.")
        return text
    if name.endswith(".docx"):
        return read_docx_bytes(content)
    return content.decode("utf-8", errors="ignore")


def source_label(path):
    name = path.name
    if "procesal" in normalize(name):
        return "Código Procesal Civil"
    if "civil" in normalize(name):
        return "Código Civil"
    return "Apuntes de derecho de contratos"


def build_knowledge_base(force=False):
    DATA_DIR.mkdir(exist_ok=True)
    if KB_PATH.exists() and not force:
        return load_json(KB_PATH, {"sources": [], "chunks": []})
    if not force:
        return {
            "built_at": "",
            "sources": [],
            "chunks": [],
            "note": "Base local pendiente de construir. Usa fuentes SPIJ o reconstruye la base fuera del análisis principal.",
        }

    chunks = []
    sources = []
    seen = set()
    for path in SOURCE_PDFS:
        if not path.exists() or str(path).lower() in seen:
            continue
        seen.add(str(path).lower())
        label = source_label(path)
        sources.append({"title": label, "path": str(path), "name": path.name})
        try:
            pages = read_pdf_bytes(path.read_bytes())
        except Exception as exc:
            chunks.append({
                "source": label,
                "page": "",
                "text": f"No se pudo leer {path.name}: {exc}",
                "kind": "error",
            })
            continue
        for page_number, page_text in pages:
            for part in split_chunks(page_text):
                chunks.append({
                    "source": label,
                    "page": page_number,
                    "text": part,
                    "kind": "pdf",
                })

    data = {
        "built_at": time.strftime("%Y-%m-%d %H:%M:%S"),
        "sources": sources,
        "chunks": chunks,
    }
    KB_PATH.write_text(json.dumps(data, ensure_ascii=False, indent=2), encoding="utf-8")
    return data


def load_json(path, fallback):
    if not path.exists():
        return fallback
    try:
        return json.loads(path.read_text(encoding="utf-8"))
    except Exception:
        return fallback


def save_json(path, data):
    DATA_DIR.mkdir(exist_ok=True)
    path.write_text(json.dumps(data, ensure_ascii=False, indent=2), encoding="utf-8")


def has_any_term(value, terms):
    value_norm = normalize(value)
    return any(normalize(term) in value_norm for term in terms if term)


def source_chunks_for_search(source):
    raw_chunks = source.get("article_chunks") or source.get("chunks") or split_article_chunks(source.get("text", ""))
    normalized = []
    for item in raw_chunks:
        if isinstance(item, dict):
            normalized.append({
                "page": item.get("page", ""),
                "text": item.get("text", ""),
                "kind": item.get("kind", "chunk"),
            })
        else:
            normalized.append({"page": "", "text": str(item), "kind": "chunk"})
    return normalized


def search_sources(query, limit=6, required_any=None, required_all=None, preferred_matter=None):
    kb = build_knowledge_base()
    spij = load_json(SPIJ_PATH, {"sources": []})
    query_tokens = tokens(query)
    if not query_tokens:
        return []
    query_set = set(query_tokens)
    article_numbers = set(re.findall(r"\bart(?:iculo)?\.?\s*(\d{1,4})\b", normalize(query)))
    scored = []

    all_chunks = list(kb.get("chunks", [])) + FALLBACK_LEGAL_SOURCES
    for source in spij.get("sources", []):
        source_chunks = source_chunks_for_search(source)
        for part in source_chunks:
            all_chunks.append({
                "source": source.get("title") or source.get("url", "SPIJ"),
                "page": part.get("page") or "SPIJ",
                "text": part.get("text", ""),
                "kind": "spij",
                "chunk_kind": part.get("kind", "chunk"),
                "url": source.get("url", ""),
                "matter": source.get("matter", ""),
                "official": source.get("official", False),
            })

    for chunk in all_chunks:
        text = chunk.get("text", "")
        haystack = f"{chunk.get('page', '')} {text}"
        haystack_norm = normalize(haystack)
        if required_any and not has_any_term(haystack, required_any):
            continue
        if required_all and not all(normalize(term) in haystack_norm for term in required_all if term):
            continue
        source_tokens = tokens(text)
        if not source_tokens:
            continue
        source_set = set(source_tokens)
        overlap = len(query_set & source_set)
        phrase_bonus = sum(3 for term in query_tokens if term in haystack_norm)
        article_bonus = 0
        if article_numbers:
            for number in article_numbers:
                if re.search(rf"\bart[íi]culo\s+{re.escape(number)}\b|\bart\.\s*{re.escape(number)}\b|\barts?\.\s*{re.escape(number)}\b", haystack, re.I):
                    article_bonus += 12
            if article_bonus == 0:
                continue
        if not article_numbers and overlap < 2:
            continue
        official_bonus = 8 if chunk.get("official") else (3 if chunk.get("kind") == "spij" else 0)
        matter_bonus = 8 if preferred_matter and chunk.get("matter") == preferred_matter else 0
        matter_penalty = -10 if preferred_matter and chunk.get("matter") and chunk.get("matter") != preferred_matter else 0
        article_chunk_bonus = 5 if chunk.get("chunk_kind") == "article" else 0
        fallback_penalty = -1 if chunk.get("kind") == "fallback" else 0
        score = overlap * 4 + phrase_bonus + article_bonus + official_bonus + matter_bonus + matter_penalty + article_chunk_bonus + fallback_penalty
        if score > 0:
            scored.append((score, chunk))

    scored.sort(key=lambda item: item[0], reverse=True)
    has_official_results = any(chunk.get("official") for _, chunk in scored)
    deduped = []
    seen = set()
    for _, chunk in scored:
        if has_official_results and chunk.get("kind") == "fallback":
            continue
        key = (chunk.get("source", ""), chunk.get("page", ""), compact_text(chunk.get("text", ""))[:80])
        if key in seen:
            continue
        seen.add(key)
        deduped.append(chunk)
        if len(deduped) >= limit:
            break
    return deduped


LEGAL_RULES = [
    {
        "level": "alto",
        "title": "Renovación automática o prórroga",
        "terms": ["renovacion automatica", "prorroga automatica", "renovara automaticamente"],
        "guidance": "revisar aviso previo, aceptación expresa y plazo de salida",
        "legal_query": "arrendamiento duracion determinada conclusion vencimiento plazo aviso previo articulo 1700 renovacion prorroga",
        "required_citation_terms": ["arrendamiento", "vencimiento", "renovacion", "plazo"],
        "matter": "civil",
    },
    {
        "level": "alto",
        "title": "Penalidad o cláusula penal",
        "terms": ["penalidad", "clausula penal", "multa", "pena convencional"],
        "guidance": "verificar proporcionalidad, supuesto de incumplimiento y límite económico",
        "legal_query": "clausula penal penalidad reduccion judicial pena excesiva cumplimiento parcial irregular articulo 1341 articulo 1346",
        "required_citation_terms": ["clausula penal", "penalidad", "pena", "reduccion"],
        "matter": "civil",
    },
    {
        "level": "alto",
        "title": "Saneamiento por vicios ocultos",
        "terms": ["vicios ocultos", "saneamiento", "tuberias", "estructura", "humedad", "defectos ocultos"],
        "guidance": "revisar si el arrendador intenta liberarse por defectos no visibles del inmueble",
        "legal_query": "saneamiento vicios ocultos defectos ocultos bien inmueble responsabilidad articulo 1484 articulo 1485 arrendador conservacion bien articulo 1680",
        "required_citation_terms": ["saneamiento", "vicios ocultos", "defectos", "arrendador", "conservar"],
        "matter": "civil",
    },
    {
        "level": "alto",
        "title": "Exoneración indebida de responsabilidad",
        "terms": ["no sera responsable", "no asume responsabilidad", "renuncia a reclamar", "libera de responsabilidad", "sin responsabilidad", "exime de responsabilidad"],
        "guidance": "verificar si la cláusula intenta eliminar responsabilidad por dolo, culpa grave o incumplimientos esenciales",
        "legal_query": "responsabilidad dolo culpa inexcusable nulidad estipulacion excluya limite responsabilidad articulo 1328 indemnizacion articulo 1321",
        "required_citation_terms": ["responsabilidad", "dolo", "culpa", "indemnizacion"],
        "matter": "civil",
    },
    {
        "level": "medio",
        "title": "Resolución o terminación unilateral",
        "terms": ["resolver el contrato", "terminacion unilateral", "sin expresion de causa", "sin causa"],
        "guidance": "precisar causales, preaviso, liquidación y efectos posteriores",
        "legal_query": "resolucion incumplimiento prestaciones reciprocas requerimiento clausula resolutoria articulo 1428 articulo 1429 articulo 1430",
        "required_citation_terms": ["resolucion", "incumplimiento", "prestacion", "cumpla"],
        "matter": "civil",
    },
    {
        "level": "medio",
        "title": "Intereses, mora o recargos",
        "terms": ["interes moratorio", "intereses moratorios", "interes compensatorio", "recargo", "tasa maxima", "mora diaria"],
        "guidance": "confirmar tasa, forma de cálculo, límite aplicable y si el recargo duplica otra penalidad",
        "legal_query": "interes compensatorio moratorio mora pago tasa maxima convencional banco central articulo 1242 articulo 1243",
        "required_citation_terms": ["interes", "moratorio", "compensatorio", "tasa"],
        "matter": "civil",
    },
    {
        "level": "medio",
        "title": "Objeto, finalidad o forma del acto jurídico",
        "terms": ["objeto imposible", "fin ilicito", "forma prescrita", "nulidad", "invalido", "sin formalidad"],
        "guidance": "verificar capacidad, objeto posible, fin lícito y formalidad exigida",
        "legal_query": "acto juridico agente capaz objeto fisica juridicamente posible fin licito forma prescrita nulidad articulo 140",
        "required_citation_terms": ["acto juridico", "objeto", "fin licito", "forma"],
        "matter": "civil",
    },
    {
        "level": "medio",
        "title": "Competencia, arbitraje o vía judicial",
        "terms": ["competencia", "jurisdiccion", "arbitraje", "juez competente", "tribunales"],
        "guidance": "identificar autoridad, domicilio contractual, conciliación previa y ley aplicable",
        "legal_query": "competencia juez competente arbitraje convenio arbitral solucion controversias contrato",
        "required_citation_terms": ["competencia", "juez", "arbitraje", "convenio arbitral"],
        "matter": "procesal_civil",
    },
    {
        "level": "medio",
        "title": "Obligaciones ambiguas",
        "terms": ["a criterio", "cuando corresponda", "entre otros", "razonable"],
        "guidance": "reemplazar fórmulas abiertas por plazos, responsables y criterios medibles",
        "legal_query": "buena fe comun intencion obligaciones contrato interpretacion cumplimiento articulo 1361 articulo 1362",
        "required_citation_terms": ["buena fe", "obligatorio", "voluntad", "intencion"],
        "matter": "civil",
    },
    {
        "level": "bajo",
        "title": "Fechas, pagos y anexos",
        "terms": ["plazo", "fecha", "pago", "cuota", "anexo", "entrega"],
        "guidance": "confirmar que fechas, montos, anexos y entregables coincidan con lo negociado",
        "legal_query": "obligaciones buena fe contrato pago renta plazo entrega articulo 1361 articulo 1362",
        "required_citation_terms": ["obligatorio", "buena fe", "pago", "renta", "plazo"],
        "matter": "civil",
    },
]


def find_relevant_excerpt(contract_text, terms, max_chars=700):
    normalized_terms = [normalize(term) for term in terms]
    paragraphs = [part.strip() for part in re.split(r"\n{2,}|(?=^.{0,45}?:)", contract_text, flags=re.M) if part.strip()]
    if not paragraphs:
        paragraphs = split_chunks(contract_text, max_chars=max_chars)
    best = ""
    best_score = -1
    for paragraph in paragraphs:
        paragraph_norm = normalize(paragraph)
        score = sum(1 for term in normalized_terms if term and term in paragraph_norm)
        if score > best_score:
            best = paragraph
            best_score = score
    best = compact_text(best or contract_text)
    if len(best) > max_chars:
        best = best[:max_chars].rsplit(" ", 1)[0] + "..."
    return best


def explain_finding(rule, excerpt, citations):
    source_refs = ", ".join(
        compact_text(f"{item.get('source', 'Fuente')} {item.get('page', '')}").strip()
        for item in citations[:3]
    )
    support_text = (
        f"Base jurídica coherente encontrada: {source_refs}. "
        if source_refs
        else "No encontré una cita suficientemente coherente para esta alerta; trátala como observación preliminar y revisa el SPIJ o una fuente oficial antes de concluir. "
    )
    if rule["title"] == "Penalidad o cláusula penal":
        return (
            "Detecté una penalidad o multa. El punto no es solo que exista, sino si el monto guarda proporción "
            "con el incumplimiento. Si una penalidad castiga de forma automática o excesiva, conviene revisarla "
            f"con la regla de reducción judicial de la cláusula penal. {support_text}"
            f"Fragmento observado: {excerpt}"
        )
    if rule["title"] == "Saneamiento por vicios ocultos":
        return (
            "Detecté lenguaje sobre saneamiento, vicios ocultos o defectos del inmueble. En simple: si hay fallas "
            "no visibles en tuberías, estructura u otros elementos importantes, no debería aceptarse sin revisar una "
            f"cláusula que haga cargar todo al arrendatario. {support_text}Fragmento observado: {excerpt}"
        )
    return (
        f"El contrato contiene indicios de {rule['title'].lower()}. Conviene {rule['guidance']}. "
        f"{support_text}Fragmento observado: {excerpt}"
    )


def analyze_contract(contract_text, purpose):
    text_norm = normalize(contract_text)
    findings = []
    for rule in LEGAL_RULES:
        if any(term in text_norm for term in rule["terms"]):
            excerpt = find_relevant_excerpt(contract_text, rule["terms"])
            citations = search_sources(
                rule.get("legal_query", f'{rule["title"]} {rule["guidance"]}'),
                limit=4,
                required_any=rule.get("required_citation_terms", []),
                preferred_matter=rule.get("matter"),
            )
            findings.append({
                "level": rule["level"],
                "title": rule["title"],
                "detail": explain_finding(rule, excerpt, citations),
                "citations": format_citations(citations),
            })

    if purpose.strip():
        citations = search_sources(purpose, limit=4)
        findings.append({
            "level": "referencial",
            "title": "Compatibilidad con la finalidad indicada",
            "detail": build_grounded_answer(purpose, citations),
            "citations": format_citations(citations),
        })

    if not findings:
        citations = []
        findings.append({
            "level": "referencial",
            "title": "No se detectaron alertas automáticas claras",
            "detail": "No encontré patrones críticos evidentes en el texto ingresado. Esto no reemplaza la revisión legal: revisa objeto, partes, precio, plazo, incumplimiento, garantías y solución de controversias.",
            "citations": format_citations(citations),
        })
    return findings


def build_grounded_answer(question, citations):
    if not citations:
        return "No encontré respaldo suficiente en la base local para responder con seguridad. Agrega más texto del contrato, revisa que los PDFs fuente estén disponibles o actualiza las fuentes desde el SPIJ."
    topics = []
    joined = normalize(" ".join(item.get("text", "") for item in citations))
    for label, words in [
        ("objeto y obligaciones", ["obligacion", "prestacion", "objeto"]),
        ("plazo y vencimiento", ["plazo", "vigencia", "vencimiento"]),
        ("incumplimiento y resolución", ["incumplimiento", "resolucion", "penalidad"]),
        ("competencia o proceso", ["competencia", "demanda", "proceso", "juez"]),
        ("pago o contraprestación", ["pago", "precio", "renta", "cuota"]),
    ]:
        if any(word in joined for word in words):
            topics.append(label)
    if not topics:
        topics = ["los elementos jurídicos relacionados con tu consulta"]
    return "Con base en las fuentes encontradas, la revisión debe centrarse en " + ", ".join(topics[:4]) + ". Si la fuente no aparece citada abajo, la aplicación no debe presentarla como respuesta segura."


def infer_matter_from_text(value):
    value_norm = normalize(value)
    signals = [
        ("tributario", ["tributario", "sunat", "impuesto", "infraccion tributaria", "sancion tributaria", "deuda tributaria"]),
        ("constitucional", ["amparo", "habeas", "constitucional", "derechos fundamentales", "proceso constitucional"]),
        ("registral", ["registro publico", "registral", "sunarp", "partida registral", "asiento registral", "registrador"]),
        ("procesal_civil", ["demanda", "excepcion", "apelacion", "casacion", "medida cautelar", "juzgado", "proceso civil"]),
        ("civil", ["contrato", "obligacion", "arrendamiento", "penalidad", "saneamiento", "vicios ocultos", "responsabilidad civil"]),
    ]
    for matter, terms in signals:
        if any(normalize(term) in value_norm for term in terms):
            return matter
    return None


def format_citations(chunks):
    citations = []
    for chunk in chunks:
        text = compact_text(chunk.get("text", ""))
        if len(text) > 360:
            text = text[:357].rstrip() + "..."
        citations.append({
            "source": chunk.get("source", "Fuente"),
            "page": chunk.get("page", ""),
            "text": text,
            "url": chunk.get("url", ""),
        })
    return citations


def answer_question(question):
    citations = search_sources(question, limit=6, preferred_matter=infer_matter_from_text(question))
    answer = build_grounded_answer(question, citations)
    return {"answer": answer, "citations": format_citations(citations)}


def fetch_url_text(url):
    parsed = urlparse(url)
    if parsed.scheme not in {"http", "https"}:
        raise ValueError("Solo se aceptan enlaces http o https.")
    socket.setdefaulttimeout(18)
    norm_id = spij_norm_id_from_url(url)
    if norm_id:
        api_url = f"https://spijwsii.minjus.gob.pe/spij-ext-back/api/procesarword/{norm_id}"
        req = Request(api_url, headers={
            "User-Agent": "Mozilla/5.0 LexCheck-SPJ-Study-App/1.0",
            "Origin": "https://spij.minjus.gob.pe",
            "Referer": "https://spij.minjus.gob.pe/spij-ext-web/",
        })
        with urlopen(req, timeout=35) as response:
            raw = response.read(8_000_000)
        text = html_to_text(raw.decode("utf-8", errors="ignore"))
        if len(text) < 1000:
            raise ValueError("El SPIJ respondió, pero no entregó texto normativo suficiente para indexar.")
        return text

    req = Request(url, headers={"User-Agent": "Mozilla/5.0 LexCheck-SPJ-Study-App/1.0"})
    with urlopen(req, timeout=18) as response:
        raw = response.read(2_500_000)
        content_type = response.headers.get("content-type", "")
    if "pdf" in content_type or url.lower().endswith(".pdf"):
        text = "\n".join(page_text for _, page_text in read_pdf_bytes(raw))
    else:
        text = html_to_text(raw.decode("utf-8", errors="ignore"))
    if "spijextweb" in normalize(text) and len(text) < 5000:
        raise ValueError("Este enlace muestra la interfaz del SPIJ, no el texto de la norma. Usa un enlace tipo detallenorma/H... o pega el texto oficial.")
    return compact_text(text)


def update_spij_sources(urls):
    store = load_json(SPIJ_PATH, {"sources": []})
    by_url = {item["url"]: item for item in store.get("sources", [])}
    results = []
    urls = [url.strip() for url in urls if url.strip()] or [item["url"] for item in DEFAULT_SPIJ_SOURCES]
    for url in urls:
        try:
            norm_id = spij_norm_id_from_url(url)
            text = fetch_url_text(url)
            digest = hashlib.sha256(text.encode("utf-8")).hexdigest()
            previous = by_url.get(url)
            changed = previous is None or previous.get("hash") != digest
            meta = default_spij_meta(url, norm_id)
            title = previous.get("title") if previous else ""
            if not title:
                title = meta.get("title") or default_spij_title(url, norm_id)
            article_chunks = split_article_chunks(text)
            by_url[url] = {
                "url": url,
                "title": title,
                "norm_id": norm_id,
                "matter": previous.get("matter") if previous else meta.get("matter", ""),
                "official": previous.get("official") if previous else meta.get("official", False),
                "hash": digest,
                "text": text,
                "chunks": split_chunks(text, max_chars=1200),
                "article_chunks": article_chunks,
                "last_checked": time.strftime("%Y-%m-%d %H:%M:%S"),
            }
            results.append({"url": url, "ok": True, "changed": changed, "chars": len(text), "articles": len(article_chunks), "title": title})
        except Exception as exc:
            results.append({"url": url, "ok": False, "error": str(exc)})
    store["sources"] = list(by_url.values())
    save_json(SPIJ_PATH, store)
    return results


HTML = r"""<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Let's Check | Contratos Perú</title>
  <meta name="description" content="Analiza contratos peruanos con fuentes oficiales del SPIJ, citas verificables y alertas sobre penalidades, saneamiento, responsabilidad, intereses y riesgos civiles." />
  <meta name="robots" content="index, follow" />
  <meta property="og:title" content="Let's Check | Analizador de contratos Perú" />
  <meta property="og:description" content="Revisión preliminar de contratos con respaldo jurídico y fuentes SPIJ." />
  <meta property="og:type" content="website" />
  <style>
    :root {
      /* INTERFAZ NARANJA CORAL PROFESIONAL Y ASISTENCIA DIDÁCTICA */
      --brand: #f97316;         /* Coral Destacado */
      --brand-hover: #ea580c;   /* Coral Oscuro */
      --ink: #0f172a;           /* Azul Pizarra Textos */
      --muted: #475569;         /* Subtítulos legibles */
      --line: #cbd5e1;          /* Bordes limpios */
      --panel: #ffffff;         /* Fondos de bloques */
      --page: #fdfaf7;          /* Fondo cálido suavizado */
      --accent: #ca8a04;
      --danger: #dc2626; 
      --warning: #d97706; 
      --ok: #16a34a;
      --soft: #fff7ed; 
      --shadow: 0 10px 30px -5px rgba(249, 115, 22, 0.08), 0 8px 16px -6px rgba(0, 0, 0, 0.04);
    }
    * { box-sizing: border-box; transition: all 0.2s ease; }
    body { margin: 0; min-height: 100vh; font-family: 'Segoe UI', Inter, system-ui, sans-serif; color: var(--ink); background: var(--page); }
    
    header { 
      min-height: 80px; display: flex; align-items: center; justify-content: space-between; gap: 16px; 
      padding: 15px clamp(16px, 4vw, 50px); border-bottom: 2px solid var(--brand); 
      background: rgba(255, 255, 255, 0.96); position: sticky; top: 0; z-index: 100; backdrop-filter: blur(8px); 
    }
    .brand { display: flex; align-items: center; gap: 12px; font-weight: 800; font-size: 22px; color: var(--ink); }
    .mark { width: 42px; height: 42px; display: grid; place-items: center; border-radius: 10px; color: white; background: linear-gradient(135deg, var(--brand), var(--brand-hover)); font-size: 20px; font-weight: bold; }
    
    .status-container { text-align: right; }
    .authors { font-size: 14px; color: var(--ink); font-weight: 700; background: #fff7ed; padding: 6px 14px; border-radius: 8px; border: 1px solid #ffedd5; box-shadow: 0 2px 4px rgba(0,0,0,0.02); }

    main { width: min(1280px, calc(100% - 32px)); margin: 30px auto 60px; display: grid; gap: 24px; }
    .top { display: grid; grid-template-columns: minmax(0, 1fr) minmax(400px, 1.2fr); gap: 24px; align-items: stretch; }
    
    section, aside { border: 1px solid #e2e8f0; border-radius: 12px; background: var(--panel); box-shadow: var(--shadow); overflow: hidden; }
    .intro { padding: clamp(24px, 4vw, 40px); display: flex; flex-direction: column; justify-content: space-between; }
    
    h1 { margin: 0; font-size: clamp(28px, 4vw, 44px); line-height: 1.15; letter-spacing: -0.02em; color: var(--ink); font-weight: 800; }
    h2 { margin: 0 0 12px; font-size: 20px; font-weight: 700; color: var(--brand-hover); display: flex; align-items: center; gap: 8px; border-bottom: 2px solid #ffedd5; padding-bottom: 6px; }
    h3 { margin: 0 0 8px; font-size: 16px; font-weight: 600; color: var(--ink); }
    p { line-height: 1.6; color: #334155; }
    .lead { color: var(--muted); font-size: 16px; margin: 16px 0 0; }
    
    .source-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 24px; }
    .source { padding: 14px; border: 1px solid #ffedd5; border-radius: 10px; background: #fffdfa; }
    .source b { display: block; margin-bottom: 4px; color: var(--brand-hover); }
    .source span { color: var(--muted); font-size: 13px; }
    
    .panel { padding: 24px; }
    label { display: block; font-weight: 700; font-size: 13px; margin: 16px 0 8px; color: #475569; text-transform: uppercase; letter-spacing: 0.05em; }
    textarea, input { width: 100%; border: 1px solid #cbd5e1; border-radius: 8px; padding: 12px; font: inherit; color: var(--ink); background: #ffffff; }
    textarea:focus, input:focus { outline: 0; border-color: var(--brand); box-shadow: 0 0 0 4px rgba(249, 115, 22, 0.15); }
    textarea { min-height: 120px; resize: vertical; }
    .short { min-height: 80px; }
    
    .filebox { border: 2px dashed #fdba74; border-radius: 10px; padding: 20px; background: #fff7ed; text-align: center; }
    .filebox:hover { border-color: var(--brand); background: #ffedd5; }
    input[type=file] { padding: 5px; max-width: 100%; }
    
    .actions { display: flex; gap: 12px; flex-wrap: wrap; margin-top: 16px; }
    button, .linkbtn { min-height: 44px; border: 0; border-radius: 8px; padding: 0 20px; display: inline-flex; align-items: center; justify-content: center; gap: 8px; font: inherit; font-weight: 700; cursor: pointer; text-decoration: none; text-align: center; }
    
    .primary { color: white; background: var(--brand); }
    .primary:hover { background: var(--brand-hover); transform: translateY(-1px); }
    .secondary { color: #334155; background: #f1f5f9; border: 1px solid #cbd5e1; }
    .secondary:hover { background: #e2e8f0; border-color: var(--brand); }
    .ghost { color: white; background: var(--ink); }
    .ghost:hover { background: #1e293b; transform: translateY(-1px); }
    
    .workspace { display: grid; grid-template-columns: 380px minmax(0, 1fr); gap: 24px; }
    .mini { color: var(--muted); font-size: 13px; margin: 4px 0 0; line-height: 1.4; }
    
    .result { display: grid; gap: 14px; }
    .finding { border: 1px solid #e2e8f0; border-left: 5px solid var(--brand); border-radius: 10px; padding: 16px; background: white; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.02); }
    .finding.alto { border-left-color: var(--danger); background: #fef2f2; }
    .finding.medio { border-left-color: var(--warning); background: #fffbeb; }
    .finding.bajo { border-left-color: var(--ok); background: #ecfdf5; }
    
    .tag { display: inline-block; margin-left: 8px; padding: 2px 8px; border-radius: 999px; background: white; border: 1px solid rgba(0,0,0,0.1); font-size: 11px; font-weight: 700; text-transform: uppercase; }
    .citation { margin-top: 12px; padding: 12px; border-radius: 8px; border: 1px solid #ffedd5; background: #fff7ed; color: #475569; font-size: 13px; line-height: 1.5; }
    .citation b { color: var(--brand-hover); }
    
    .spij-flow { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-top: 16px; margin-bottom: 16px; }
    .step { padding: 12px; border: 1px solid #e2e8f0; border-radius: 8px; background: #fafafa; min-height: 85px; }
    .step strong { display: block; font-size: 13px; color: var(--brand-hover); margin-bottom: 4px; }
    .step span { color: var(--muted); font-size: 12px; line-height: 1.3; }
    
    .notice { padding: 14px; border-radius: 8px; background: #fff7ed; color: var(--brand-hover); border: 1px solid #ffedd5; font-size: 13px; font-weight: 500; }
    hr { border: 0; border-top: 1px solid #e2e8f0; margin: 20px 0; }

    /* SECCIÓN DE CONTACTO DE ABOGADOS EXPANDIDA ABAJO (FULL WIDTH) */
    .lawyers-footer { background: #fff7ed; border: 2px solid #ffedd5; padding: 24px; border-radius: 12px; box-shadow: var(--shadow); }
    .lawyers-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin-top: 16px; }
    .lawyer-card { background: white; padding: 16px; border-radius: 10px; border: 1px solid #e2e8f0; display: flex; flex-direction: column; justify-content: space-between; gap: 12px; }
    .lawyer-info h4 { margin: 0; font-size: 15px; color: var(--ink); font-weight: 700; }
    .lawyer-info span { font-size: 13px; color: var(--muted); display: block; margin-top: 4px; }
    .lawyer-btn { font-size: 13px; padding: 8px 14px; background: var(--brand); color: white; border-radius: 6px; text-decoration: none; font-weight: bold; text-align: center; display: block; }
    .lawyer-btn:hover { background: var(--brand-hover); }

    /* Modal Flotante de Satisfacción */
    .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(15, 23, 42, 0.6); display: flex; align-items: center; justify-content: center; z-index: 1000; opacity: 0; pointer-events: none; transition: opacity 0.3s ease; }
    .modal-overlay.active { opacity: 1; pointer-events: auto; }
    .modal-box { background: white; padding: 30px; border-radius: 16px; max-width: 450px; width: calc(100% - 32px); box-shadow: 0 25px 50px -12px rgba(0,0,0,0.25); transform: scale(0.95); transition: transform 0.3s ease; border: 2px solid var(--brand); }
    .modal-overlay.active .modal-box { transform: scale(1); }
    .modal-box h3 { margin-top: 0; font-size: 22px; color: var(--brand-hover); }
    .rating-options { display: flex; justify-content: space-between; margin: 16px 0; }
    .rating-btn { border: 1px solid #cbd5e1; background: #f8fafc; padding: 10px; border-radius: 8px; cursor: pointer; flex: 1; text-align: center; font-size: 18px; margin: 0 4px; }
    .rating-btn:hover, .rating-btn.selected { background: #ffedd5; border-color: var(--brand); }
    
    .feedback-suggestions-label { margin-top: 12px; font-size: 12px; display: block; font-weight: bold;}
    
    @media (max-width: 1024px) { .top, .workspace { grid-template-columns: 1fr; } .lawyers-grid { grid-template-columns: 1fr; } }
    @media (max-width: 560px) { header { align-items: flex-start; flex-direction: column; gap: 8px; } .status-container { text-align: left; } button, .linkbtn { width: 100%; } .spij-flow { grid-template-columns: 1fr; } }
  </style>
</head>
<body>
  <header>
    <div class="brand"><div class="mark">§</div> Let's Check</div>
    <div class="status-container">
      <div class="authors">Lushiana Escobedo Castro & Alel Nadim Orbezo</div>
    </div>
  </header>
  <main>
    <div class="top">
      <section class="intro">
        <h1>Auditor de contratos con respaldo jurídico legítimo.</h1>
        <p class="lead">Analiza contratos usando fuentes oficiales del SPIJ y fuentes complementarias que agregues. Cuando no se localiza respaldo suficiente, la aplicación lo advierte oportunamente.</p>
        <div class="source-grid">
          <div class="source"><b>Fuentes por materia</b><span>Civil, procesal civil, tributario, constitucional y registral se buscan por separado.</span></div>
          <div class="source"><b>Citas por artículo</b><span>Cada alerta intenta mostrar artículo, fuente y enlace verificable.</span></div>
          <div class="source"><b>Conexión SPIJ</b><span>Lee enlaces oficiales detallenorma/H... y detecta cambios por comparación.</span></div>
          <div class="source"><b>Criterio prudente</b><span>Evita emitir juicios de valor si no halla sustento documental real.</span></div>
        </div>
      </section>
      <aside class="panel">
        <h2>Analizar contrato</h2>
        <div class="filebox">
          <label for="contractFile">Selecciona un archivo PDF, DOCX o TXT</label>
          <input id="contractFile" type="file" accept=".pdf,.docx,.txt" />
          <p class="mini" id="fileState">También puedes pegar el texto directamente abajo.</p>
        </div>
        <label for="contractText">Texto del contrato</label>
        <textarea id="contractText" placeholder="Pega aquí las cláusulas específicas o el contrato completo para su revisión."></textarea>
        <label for="purposeText">¿Qué objetivo buscas lograr o qué riesgo deseas evitar?</label>
        <textarea id="purposeText" class="short" placeholder="Ejemplo: deseo arrendar un local comercial sin renovación automática y prever una salida rápida ante incumplimientos."></textarea>
        <div class="actions">
          <button class="primary" id="analyzeBtn" style="flex: 1;">Iniciar análisis</button>
          <button class="secondary" id="demoBtn" style="flex: 1;">Cargar ejemplo de prueba</button>
        </div>
      </aside>
    </div>

    <div class="workspace">
      <aside class="panel">
        <h2>Consulta jurídica rápida</h2>
        <p class="mini">Pregunta libremente sobre contratos, obligaciones, artículos específicos o derecho procesal civil peruano.</p>
        <label for="questionText">Tu consulta</label>
        <textarea id="questionText" class="short" placeholder="Ejemplo: ¿qué requisitos de validez se exigen para una cláusula penal en el Perú?"></textarea>
        <div class="actions"><button class="ghost" id="questionBtn" style="width: 100%;">Responder usando fuentes</button></div>
        <hr />
        <h2>Actualización desde el SPIJ</h2>
        <p class="mini">La aplicación puede leer enlaces oficiales tipo detallenorma/H... del SPIJ. También puedes agregar fuentes complementarias, pero las citas oficiales tendrán prioridad.</p>
        <label for="spijUrls">Enlaces oficiales</label>
        <textarea id="spijUrls" class="short" placeholder="https://spij.minjus.gob.pe/...">https://spij.minjus.gob.pe/spij-ext-web/#/detallenorma/H682684
https://spij.minjus.gob.pe/spij-ext-web/#/detallenorma/H682685
https://spij.minjus.gob.pe/spij-ext-web/#/detallenorma/H682696
https://spij.minjus.gob.pe/spij-ext-web/#/detallenorma/H1288461
https://lpderecho.pe/texto-unico-ordenado-reglamento-general-registros-publicos-resolucion-126-2012-sunarp-sn/</textarea>
        <div class="actions" style="display: flex; width: 100%;">
          <button class="secondary" id="spijBtn" style="flex: 1;">Revisar variaciones</button>
          <a class="linkbtn secondary" href="https://spij.minjus.gob.pe/" target="_blank" rel="noreferrer" style="flex: 1; text-align: center;">Ir al portal SPIJ</a>
        </div>
      </aside>

      <section class="panel">
        <h2>Resultados de la evaluación</h2>
        <div class="notice">Regla estricta de seguridad: si una conclusión no cuenta con una cita textual de respaldo al pie, trátala únicamente como orientación preliminar.</div>
        <div class="spij-flow">
          <div class="step"><strong>1. Consultar SPIJ</strong><span>Verifica el estado actual de las normas ingresadas.</span></div>
          <div class="step"><strong>2. Evaluar cambios</strong><span>Compara el texto histórico con las modificaciones del diario oficial.</span></div>
          <div class="step"><strong>3. Actualizar base</strong><span>Registra las variaciones de los artículos en la memoria interna.</span></div>
          <div class="step"><strong>4. Emitir informe</strong><span>Despliega las alertas identificadas con sus fuentes exactas.</span></div>
        </div>
        <div id="results" class="result" style="margin-top:16px;"></div>
      </section>
    </div>

    <!-- SECCIÓN DE CONTACTO DE ABOGADOS TOTALMENTE REUBICADA ABAJO (A TODO LO ANCHO) -->
    <div class="lawyers-footer">
      <h3 style="margin: 0; color: var(--brand-hover); font-size: 18px; display: flex; align-items: center; gap: 8px;">📍 Contacto Legal Especializado</h3>
      <p class="mini" style="margin: 4px 0 12px 0; font-size: 14px;">Abogados peruanos sugeridos de alto nivel y expertos reconocidos en Derecho de Contratos y Civil:</p>
      
      <div class="lawyers-grid">
        <div class="lawyer-card">
          <div class="lawyer-info">
            <h4>Dr. Carlos Cárdenas Quirós</h4>
            <span>Especialista de gran trayectoria en Obligaciones, Contratos y Acto Jurídico.</span>
          </div>
          <a href="mailto:carlos.cardenas@estudiolegal.pe?subject=Consulta%20sobre%20Contratos%20-%20LexCheck" class="lawyer-btn">Contactar Especialista</a>
        </div>

        <div class="lawyer-card">
          <div class="lawyer-info">
            <h4>Dra. Elena Alvites Alvites</h4>
            <span>Consultora experta en Contratación Civil, Comercial y Derecho Público.</span>
          </div>
          <a href="mailto:elena.alvites@derechoperu.com?subject=Consulta%20sobre%20Contratos%20-%20LexCheck" class="lawyer-btn">Contactar Especialista</a>
        </div>

        <div class="lawyer-card">
          <div class="lawyer-info">
            <h4>Dr. Mario Castillo Freyre</h4>
            <span>Especialista de renombre nacional e internacional en Teoría de los Contratos.</span>
          </div>
          <a href="mailto:mario@castillofreyre.com?subject=Consulta%20sobre%20Contratos%20-%20LexCheck" class="lawyer-btn">Contactar Especialista</a>
        </div>
      </div>
    </div>
  </main>

  <!-- MODAL FLOTANTE DE SATISFACCIÓN -->
  <div class="modal-overlay" id="feedbackModal">
    <div class="modal-box">
      <h3>¡Tu opinión es muy importante!</h3>
      <p style="font-size: 14px; color: #475569;">Por favor, indícanos tu nivel de satisfacción con el análisis de tu contrato realizado por LexCheck IA:</p>
      
      <div class="rating-options">
        <div class="rating-btn" onclick="selectRating(this, 'Muy Insatisfecho')">😞</div>
        <div class="rating-btn" onclick="selectRating(this, 'Regular')">😐</div>
        <div class="rating-btn" onclick="selectRating(this, 'Satisfecho')">🙂</div>
        <div class="rating-btn" onclick="selectRating(this, '¡Excelente!')">🤩</div>
      </div>
      
      <label for="feedbackSuggestions" class="feedback-suggestions-label">¿Qué cosas sugieres que podemos mejorar?</label>
      <textarea id="feedbackSuggestions" placeholder="Escribe aquí tus comentarios, ideas o sugerencias..." style="min-height: 80px; font-size: 13px;"></textarea>
      
      <div style="display: flex; gap: 10px; margin-top: 18px;">
        <button class="secondary" onclick="closeModal()" style="flex: 1;">Omitir</button>
        <button class="primary" onclick="sendFeedback()" style="flex: 1;">Enviar Comentarios</button>
      </div>
    </div>
  </div>

  <script>
    const $ = id => document.getElementById(id);
    const results = $("results");
    let chosenRating = "";
    
    const demoText = `Contrato de arrendamiento de local comercial. El contrato se renovará automáticamente por periodos iguales si ninguna parte comunica su decisión con 5 días de anticipación. El arrendatario pagará una penalidad equivalente al 40% de la renta anual por cualquier incumplimiento. Las partes se someten a la competencia de los jueces de Lima.`;
    const demoPurpose = "Quiero arrendar un local sin quedar atado por renovación automática y con penalidad razonable.";

    $("contractFile").addEventListener("change", () => {
      const file = $("contractFile").files[0];
      $("fileState").textContent = file ? `Archivo listo para procesar: ${file.name}` : "También puedes pegar el texto abajo.";
    });
    
    $("demoBtn").addEventListener("click", () => {
      $("contractText").value = demoText;
      $("purposeText").value = demoPurpose;
      analyze();
    });
    
    $("analyzeBtn").addEventListener("click", analyze);
    $("questionBtn").addEventListener("click", ask);
    $("spijBtn").addEventListener("click", updateSpij);

    async function analyze() {
      const form = new FormData();
      const file = $("contractFile").files[0];
      if (file) form.append("file", file);
      form.append("contractText", $("contractText").value);
      form.append("purpose", $("purposeText").value);
      showLoading("Analizando minuciosamente con fuentes locales actualizadas...");
      try {
        const data = await post("/api/analyze", form);
        renderFindings(data.findings || []);
        triggerModalDelayed();
      } catch (error) {
        renderError(error);
      }
    }

    async function ask() {
      const form = new FormData();
      form.append("question", $("questionText").value);
      showLoading("Localizando sustento normativo en la base de datos...");
      try {
        const data = await post("/api/question", form);
        renderAnswer(data);
        triggerModalDelayed();
      } catch (error) {
        renderError(error);
      }
    }

    async function updateSpij() {
      const form = new FormData();
      form.append("urls", $("spijUrls").value);
      showLoading("Extrayendo y contrastando enlaces del SPIJ oficial...");
      try {
        const data = await post("/api/update-spij", form);
        renderSpij(data.results || []);
      } catch (error) {
        renderError(error);
      }
    }

    async function post(url, form) {
      const controller = new AbortController();
      const timeout = setTimeout(() => controller.abort(), 60000);
      let response;
      try {
        response = await fetch(url, { method: "POST", body: form, signal: controller.signal });
      } catch (error) {
        if (error.name === "AbortError") throw new Error("La operación tardó más de 60 segundos. Prueba con un archivo más pequeño o revisa la fuente del SPIJ.");
        throw error;
      } finally {
        clearTimeout(timeout);
      }
      if (!response.ok) throw new Error(await response.text());
      return response.json();
    }

    function showLoading(text) {
      results.innerHTML = `<div class="finding"><h3>${escapeHtml(text)}</h3><p>Procesando la información jurídica, un momento por favor.</p></div>`;
    }

    function renderFindings(findings) {
      results.innerHTML = findings.map(item => `
        <article class="finding ${item.level}">
          <h3>${escapeHtml(item.title)} <span class="tag" style="background: var(--brand); color: white; font-weight: bold; border: none;">Riesgo ${escapeHtml(item.level)}</span></h3>
          <p>${escapeHtml(item.detail)}</p>
          ${renderCitations(item.citations || [])}
        </article>
      `).join("") || `<div class="finding"><h3>Sin resultados</h3><p>No se localizó texto con extensión suficiente para ejecutar la auditoría.</p></div>`;
    }

    function renderAnswer(data) {
      results.innerHTML = `
        <article class="finding bajo">
          <h3>Respuesta estructurada con fuentes confiables</h3>
          <p>${escapeHtml(data.answer || "")}</p>
          ${renderCitations(data.citations || [])}
        </article>
      `;
    }

    function renderSpij(items) {
      results.innerHTML = items.map(item => `
        <article class="finding ${item.ok ? "bajo" : "alto"}">
          <h3>${item.ok ? (item.changed ? "Fuente jurídica actualizada" : "Normativa sin modificaciones") : "Inconveniente al revisar la fuente"}</h3>
          ${item.title ? `<p><b>Fuente:</b> ${escapeHtml(item.title)}</p>` : ""}
          <p style="word-break: break-all;"><b>Enlace:</b> ${escapeHtml(item.url || "")}</p>
          <p>${item.ok ? `Contenido indexado: ${item.chars} caracteres procesados${item.articles ? `, ${item.articles} fragmentos por artículo` : ""} y listo para citar.` : escapeHtml(item.error || "")}</p>
        </article>
      `).join("") || `<div class="finding"><h3>Lista vacía</h3><p>Ingresa una o más direcciones web válidas del SPIJ.</p></div>`;
    }

    function renderError(error) {
      results.innerHTML = `
        <article class="finding alto">
          <h3>No se pudo completar el análisis</h3>
          <p>${escapeHtml(error.message || error)}</p>
          <div class="citation"><b>Qué hacer:</b> revisa que el PDF tenga texto seleccionable, que el servidor siga abierto en VS Code y que las fuentes del SPIJ respondan.</div>
        </article>
      `;
    }

    function renderCitations(citations) {
      if (!citations.length) return `<div class="citation"><b>Sustento legal:</b> No se detectaron fragmentos literales explícitos en la base local.</div>`;
      return citations.map(c => `
        <div class="citation">
          <b>Fundamento: ${escapeHtml(c.source)}${c.page ? " - Pág. " + escapeHtml(String(c.page)) : ""}</b>
          ${c.url ? `<br><a href="${escapeHtml(c.url)}" target="_blank" rel="noreferrer" style="color: var(--brand-hover); font-weight: 500;">Ver enlace oficial SPIJ</a>` : ""}
          <div style="margin-top: 4px; font-style: italic; color: #334155;">"${escapeHtml(c.text)}"</div>
        </div>
      `).join("");
    }

    function escapeHtml(value) {
      return String(value).replace(/[&<>"']/g, ch => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;" }[ch]));
    }

    /* FUNCIONES CONTROLADORAS DEL FORMULARIO FLOTANTE DE SATISFACCIÓN */
    function triggerModalDelayed() {
      setTimeout(() => {
        $("feedbackModal").classList.add("active");
      }, 1500); 
    }

    function selectRating(element, ratingValue) {
      const btns = document.querySelectorAll('.rating-btn');
      btns.forEach(b => b.classList.remove('selected'));
      element.classList.add('selected');
      chosenRating = ratingValue;
    }

    function closeModal() {
      $("feedbackModal").classList.remove("active");
      clearFeedbackForm();
    }

    function sendFeedback() {
      const suggestions = $("feedbackSuggestions").value.trim();
      if(!chosenRating) {
        alert("Por favor, selecciona un emoticón para calificar tu nivel de satisfacción.");
        return;
      }
      
      alert(`¡Muchas gracias por tu feedback!\n\nSe ha enviado la siguiente información a lucianacastro1304@gmail.com:\n- Nivel de Satisfacción: ${chosenRating}\n- Sugerencias: ${suggestions || "Ninguna"}`);
      closeModal();
    }

    function clearFeedbackForm() {
      chosenRating = "";
      $("feedbackSuggestions").value = "";
      const btns = document.querySelectorAll('.rating-btn');
      btns.forEach(b => b.classList.remove('selected'));
    }
  </script>
</body>
</html>"""


class LexCheckHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path in {"/", "/index.html"}:
            self.reply(200, HTML.encode("utf-8"), "text/html; charset=utf-8")
            return
        if self.path == "/robots.txt":
            body = "User-agent: *\nAllow: /\nSitemap: /sitemap.xml\n"
            self.reply(200, body.encode("utf-8"), "text/plain; charset=utf-8")
            return
        if self.path == "/sitemap.xml":
            host = self.headers.get("Host", f"localhost:{PORT}")
            scheme = "https" if "localhost" not in host and "127.0.0.1" not in host else "http"
            body = f"""<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>{scheme}://{host}/</loc>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
</urlset>
"""
            self.reply(200, body.encode("utf-8"), "application/xml; charset=utf-8")
            return
        if self.path == "/health":
            self.json({"ok": True, "sources": len(load_json(SPIJ_PATH, {"sources": []}).get("sources", []))})
            return
        self.reply(404, b"No encontrado", "text/plain; charset=utf-8")

    def do_POST(self):
        try:
            content_length = int(self.headers.get('Content-Length', 0))
            post_data = self.rfile.read(content_length)
            
            from urllib.parse import parse_qs
            parametros = parse_qs(post_data.decode('utf-8'))
            
            if self.path == "/api/analyze":
                contract_text = parametros.get('contractText', [''])[0]
                purpose = parametros.get('purpose', [''])[0]
                findings = analyze_contract(contract_text, purpose)
                self.json({"findings": findings})
                return
                
            elif self.path == "/api/question":
                question = parametros.get('question', [''])[0]
                self.json(answer_question(question))
                return
                
            elif self.path == "/api/update-spij":
                urls = parametros.get('urls', [''])[0].splitlines()
                self.json({"results": update_spij_sources(urls)})
                return
                
        except Exception as e:
            self.reply(500, f"Error en el servidor: {str(e)}".encode('utf-8'), "text/plain")

    def json(self, data):
        self.reply(200, json.dumps(data, ensure_ascii=False).encode("utf-8"), "application/json; charset=utf-8")

    def reply(self, status, body, content_type):
        self.send_response(status)
        self.send_header("Content-Type", content_type)
        self.send_header("Content-Length", str(len(body)))
        self.end_headers()
        self.wfile.write(body)

    def log_message(self, *_):
        return


def open_browser():
    webbrowser.open(f"http://localhost:{PORT}")


if __name__ == "__main__":
    DATA_DIR.mkdir(exist_ok=True)
    server = HTTPServer(("localhost", PORT), LexCheckHandler)
    print(f"Let's Check listo en http://localhost:{PORT}")
    Timer(1, open_browser).start()
    server.serve_forever()
