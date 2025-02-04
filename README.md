# Soon to be a proxy site
import requests

app = Flask(__name__)

SEARCH_ENGINE_URL = "https://api.duckduckgo.com/"

@app.route('/search', methods=['GET'])
def search():
    query = request.args.get('q')
    if not query:
        return jsonify({"error": "Query parameter 'q' is required"}), 400

    # Perform the search request to DuckDuckGo
    params = {
        'q': query,
        'format': 'json'
    }
    response = requests.get(SEARCH_ENGINE_URL, params=params)

    if response.status_code != 200:
        return jsonify({"error": "Failed to fetch search results"}), response.status_code

    return jsonify(response.json())

if __name__ == '__main__':
    app.run(debug=True)
