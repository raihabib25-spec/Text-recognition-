# Text-recognition-
# 𝗔 𝗰𝗼𝗺𝗽𝗹𝗲𝘁𝗲 𝗰𝗼𝗱𝗲 𝗳𝗼𝗿 𝘁𝗲𝘅𝘁 𝗿𝗲𝗰𝗼𝗴𝗻𝗶𝘁𝗶𝗼𝗻

import re
import long_responses as long


def message_probability(user_message, recognised_words, single_response=False, required_words=[]):
    message_certainty = 0
    has_required_words = True

    # 𝗖𝗼𝘂𝗻𝘁𝘀 𝗵𝗼𝘄 𝗺𝗮𝗻𝘆 𝘄𝗼𝗿𝗱𝘀 𝗮𝗿𝗲 𝗽𝗿𝗲𝘀𝗲𝗻𝘁 𝗶𝗻 𝗲𝗮𝗰𝗵 𝗽𝗿𝗲𝗱𝗲𝗳𝗶𝗻𝗲𝗱 𝗺𝗲𝘀𝘀𝗮𝗴𝗲
    for word in user_message:
        if word in recognised_words:
            message_certainty += 1

    # 𝗖𝗮𝗹𝗰𝘂𝗹𝗮𝘁𝗲𝘀 𝘁𝗵𝗲 𝗽𝗲𝗿𝗰𝗲𝗻𝘁 𝗼𝗳 𝗿𝗲𝗰𝗼𝗴𝗻𝗶𝘀𝗲𝗱 𝘄𝗼𝗿𝗱𝘀 𝗶𝗻 𝗮 𝘂𝘀𝗲𝗿 𝗺𝗲𝘀𝘀𝗮𝗴𝗲
    percentage = float(message_certainty) / float(len(recognised_words))

    # 𝗖𝗵𝗲𝗰𝗸𝘀 𝘁𝗵𝗮𝘁 𝘁𝗵𝗲 𝗿𝗲𝗾𝘂𝗶𝗿𝗲𝗱 𝘄𝗼𝗿𝗱𝘀 𝗮𝗿𝗲 𝗶𝗻 𝘁𝗵𝗲 𝘀𝘁𝗿𝗶𝗻𝗴
    for word in required_words:
        if word not in user_message:
            has_required_words = False
            break

    # 𝗠𝘂𝘀𝘁 𝗲𝗶𝘁𝗵𝗲𝗿 𝗵𝗮𝘃𝗲 𝘁𝗵𝗲 𝗿𝗲𝗾𝘂𝗶𝗿𝗲𝗱 𝘄𝗼𝗿𝗱𝘀, 𝗼𝗿 𝗯𝗲 𝗮 𝘀𝗶𝗻𝗴𝗹𝗲 𝗿𝗲𝘀𝗽𝗼𝗻𝘀𝗲
    if has_required_words or single_response:
        return int(percentage * 100)
    else:
        return 0


def check_all_messages(message):
    highest_prob_list = {}

    # 𝗦𝗶𝗺𝗽𝗹𝗶𝗳𝗶𝗲𝘀 𝗿𝗲𝘀𝗽𝗼𝗻𝘀𝗲 𝗰𝗿𝗲𝗮𝘁𝗶𝗼𝗻 / 𝗮𝗱𝗱𝘀 𝗶𝘁 𝘁𝗼 𝘁𝗵𝗲 𝗱𝗶𝗰𝘁
    def response(bot_response, list_of_words, single_response=False, required_words=[]):
        nonlocal highest_prob_list
        highest_prob_list[bot_response] = message_probability(message, list_of_words, single_response, required_words)

    # 𝗥𝗲𝘀𝗽𝗼𝗻𝘀𝗲𝘀 -------------------------------------------------------------------------------------------------------
    response('Hello!', ['hello', 'hi', 'hey', 'sup', 'heyo'], single_response=True)
    response('See you!', ['bye', 'goodbye'], single_response=True)
    response('I\'m doing fine, and you?', ['how', 'are', 'you', 'doing'], required_words=['how'])
    response('You\'re welcome!', ['thank', 'thanks'], single_response=True)
    response('Thank you!', ['i', 'love', 'code', 'palace'], required_words=['code', 'palace'])

    # 𝗟𝗼𝗻𝗴𝗲𝗿 𝗿𝗲𝘀𝗽𝗼𝗻𝘀𝗲𝘀
    response(long.R_ADVICE, ['give', 'advice'], required_words=['advice'])
    response(long.R_EATING, ['what', 'you', 'eat'], required_words=['you', 'eat'])

    best_match = max(highest_prob_list, key=highest_prob_list.get)
     print(highest_prob_list)
     print(f'Best match = {best_match} | Score: {highest_prob_list[best_match]}')

    return long.unknown() if highest_prob_list[best_match] < 1 else best_match


# Used to get the response
def get_response(user_input):
    split_message = re.split(r'\s+|[,;?!.-]\s*', user_input.lower())
    response = check_all_messages(split_message)
    return response


# Testing the response system
while True:
    print('Bot: ' + get_response(input('You: ')))
