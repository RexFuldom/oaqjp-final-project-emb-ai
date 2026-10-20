import requests
import json

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    headers = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    input_json = { "raw_document": { "text": text_to_analyze } }
    
    # Realizar la solicitud POST a la API de Watson NLP
    response = requests.post(url, json=input_json, headers=headers)
    
    # Convertir la respuesta de texto a un diccionario usando la librería json
    formatted_response = json.loads(response.text)
    
    # Extraer las emociones y sus puntuaciones
    emotion_predictions = formatted_response['emotionPredictions'][0]['emotion']
    anger_score = emotion_predictions['anger']
    disgust_score = emotion_predictions['disgust']
    fear_score = emotion_predictions['fear']
    joy_score = emotion_predictions['joy']
    sadness_score = emotion_predictions['sadness']
    
    # Crear un diccionario con todas las puntuaciones de emociones
    emotion_scores = {
        'anger': anger_score,
        'disgust': disgust_score,
        'fear': fear_score,
        'joy': joy_score,
        'sadness': sadness_score
    }
    
    # Encontrar la emoción dominante (la que tiene la puntuación más alta)
    dominant_emotion = max(emotion_scores, key=emotion_scores.get)
    
    # Devolver el formato de salida requerido
    return {
        'anger': anger_score,
        'disgust': disgust_score,
        'fear': fear_score,
        'joy': joy_score,
        'sadness': sadness_score,
        'dominant_emotion': dominant_emotion
    }