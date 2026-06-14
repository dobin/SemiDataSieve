# Flask Integration Guide for SemiDataSieve

The `index.html` file works standalone with demo data, but can be easily integrated with Flask using a REST API endpoint (recommended) or template injection for small datasets.

## Quick Setup

### Method 1: REST API (Recommended)

**Step 1:** Create your Flask app with API endpoint:

```python
from flask import Flask, render_template, jsonify

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html', api_endpoint='/api/logs')

@app.route('/api/logs')
def api_logs():
    data = get_your_log_data()  # Your data source
    return jsonify(data)
```

**Step 2:** In `templates/index.html`, uncomment this line (around line 283):
```javascript
const FLASK_API_ENDPOINT_VAR = "{{ api_endpoint }}";
```

Done! The page auto-fetches data from `/api/logs` on load.

---

### Method 2: Template Injection (Small Datasets Only)

For small datasets (<1000 entries), uncomment in `templates/index.html`:
```javascript
const FLASK_DATA = {{ data | tojson | safe }};
```

Then pass data directly: `render_template('index.html', data=your_data)`

---

### Optional: Pre-configure UI

Pass optional configuration in `render_template()`:
```python
filters = [{"type": "key", "action": "exclude", "key": "debug", "value": "", "active": True}]
visibility = [{"type": "key", "action": "header", "key": "timestamp", "value": ""}]
color_rules = [{"type": "value", "match": "ERROR", "color": "white", "backgroundColor": "#d9534f"}]

return render_template('index.html', api_endpoint='/api/logs', 
                      filters=filters, visibility=visibility, color_rules=color_rules)
```

Then uncomment in HTML:
```javascript
const FLASK_FILTERS = {{ filters | tojson | safe }};
const FLASK_VISIBILITY = {{ visibility | tojson | safe }};
const FLASK_COLOR_RULES = {{ color_rules | tojson | safe }};
```

## Complete Working Example

```python
from flask import Flask, render_template, jsonify

app = Flask(__name__)

@app.route('/')
def index():
    # Optional: pre-configure UI
    visibility = [
        {"type": "key", "action": "header", "key": "timestamp", "value": ""},
        {"type": "key", "action": "header", "key": "level", "value": ""}
    ]
    
    color_rules = [
        {"type": "value", "match": "ERROR", "color": "white", "backgroundColor": "#d9534f"},
        {"type": "value", "match": "WARNING", "color": "black", "backgroundColor": "#ffc107"}
    ]
    
    return render_template('index.html', 
                         api_endpoint='/api/logs',
                         visibility=visibility,
                         color_rules=color_rules)

@app.route('/api/logs')
def api_logs():
    # Your data source here (database, file, etc.)
    data = [
        {"id": 1, "timestamp": "2025-03-08 12:00:00", "level": "INFO", "message": "System started"},
        {"id": 2, "timestamp": "2025-03-08 12:05:00", "level": "ERROR", "message": "System crash"}
    ]
    return jsonify(data)

if __name__ == '__main__':
    app.run(debug=True)
```

## Common Issues

- **No data showing**: Check browser console for errors. Verify API endpoint path matches Flask route.
- **JSON errors**: Use `.isoformat()` for datetime objects: `{"timestamp": datetime.now().isoformat()}`
- **CORS errors**: Install `flask-cors` if API is on different domain.
- **Large datasets slow**: Use pagination or server-side filtering in your API endpoint.
