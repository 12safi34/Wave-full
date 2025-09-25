// wavefull bot basic code
// Dependencies: telegraf
// Install: npm install telegraf

const { Telegraf } = require('telegraf');

const bot = new Telegraf(process.env.BOT_TOKEN);

// In-memory storage (DB এর জায়গায় আপাতত)
const users = {}; // { telegram_id: {balance: number, referred_by: id|null} }

// helper: add coins
function addCoins(userId, amount) {
  if (!users[userId]) {
    users[userId] = { balance: 0, referred_by: null };
  }
  users[userId].balance += amount;
}

bot.start((ctx) => {
  const userId = ctx.from.id;
  const args = ctx.startPayload; // referral id if any

  if (!users[userId]) {
    users[userId] = { balance: 0, referred_by: null };
    // নতুন ইউজার রেজিস্ট্রেশন বোনাস
    addCoins(userId, 10);

    // referral handle
    if (args && args.startsWith("ref")) {
      const refId = args.replace("ref", "");
      if (users[refId] && refId != userId) {
        addCoins(refId, 5); // রেফারার বোনাস
        users[userId].referred_by = refId;
        ctx.reply(`আপনি সফলভাবে wavefull এ যোগ দিলেন ✅ \nআপনার রেফারার কিছু কয়েন পেয়েছে!`);
      }
    }
  }

  ctx.reply(
    `👋 স্বাগতম Wavefull Bot এ!\n\n` +
    `আপনার referral লিঙ্ক:\n` +
    `https://t.me/${ctx.botInfo.username}?start=ref${userId}\n\n` +
    `👉 /balance লিখে আপনার কয়েন দেখুন।`
  );
});

bot.command("balance", (ctx) => {
  const userId = ctx.from.id;
  if (!users[userId]) {
    users[userId] = { balance: 0, referred_by: null };
  }
  ctx.reply(`💰 আপনার balance: ${users[userId].balance} কয়েন`);
});

bot.launch();
console.log("Wavefull bot চলছে...");
