import Stripe from "stripe";

export default async function handler(req, res) {
  const stripe = new Stripe(process.env.STRIPE_SECRET_KEY);

  try {
    const session = await stripe.checkout.sessions.create({
      mode: "subscription",
      payment_method_types: ["card"],
      line_items: [
        {
          price: req.body.priceId,
          quantity: 1,
        }
      ],
      success_url: "https://seuapp.com/sucesso",
      cancel_url: "https://seuapp.com/cancelado"
    });

    res.status(200).json({ url: session.url });

  } catch (error) {
    console.error(error);
    res.status(500).json({ error: error.message });
  }
}
