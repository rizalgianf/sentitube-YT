@app.route('/search_videos', methods=['POST'])
def search_videos():
    data = request.json
    query = data.get('query')
    if not query:
        return jsonify({'error': 'query is required'}), 400
    youtube = build('youtube', 'v3', developerKey=YOUTUBE_API_KEY)
    search_response = youtube.search().list(
        q=query,
        part='snippet',
        type='video',
        maxResults=10
    ).execute()
    results = []
    for item in search_response.get('items', []):
        results.append({
            'video_id': item['id']['videoId'],
            'title': item['snippet']['title'],
            'channel_title': item['snippet']['channelTitle'],
            'published_at': item['snippet']['publishedAt'],
            'thumbnail': item['snippet']['thumbnails']['high']['url']
        })
    return jsonify(results)

    INI YANG NGGA PERLU JIKA NGGA PAKE SEARCH 
