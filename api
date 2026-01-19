export const config = {
  runtime: 'edge',
};

export default async function handler(req) {
  if (req.method !== 'POST') {
    return new Response('Method not allowed', { status: 405 });
  }

  // Vercel Environment Variables থেকে গোপন তথ্য নেওয়া
  const BOT_TOKEN = process.env.BOT_TOKEN; 
  const CHAT_ID = process.env.CHAT_ID;

  try {
    const formData = await req.formData();
    const type = formData.get('type'); 
    
    // টেলিগ্রাম API সেটআপ
    const method = type === 'document' ? 'sendDocument' : 'sendMessage';
    const telegramUrl = `https://api.telegram.org/bot${BOT_TOKEN}/${method}`;

    // চ্যাট আইডি যুক্ত করা
    if (!formData.has('chat_id')) {
      formData.append('chat_id', CHAT_ID);
    }
    
    formData.delete('type');

    // টেলিগ্রামে পাঠানো
    const response = await fetch(telegramUrl, {
      method: 'POST',
      body: formData,
    });

    const data = await response.json();
    return new Response(JSON.stringify(data), {
      status: 200,
      headers: { 'Content-Type': 'application/json' }
    });

  } catch (error) {
    return new Response(JSON.stringify({ error: 'Server Error' }), { status: 500 });
  }
}
