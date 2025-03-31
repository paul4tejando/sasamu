import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { motion } from "framer-motion";

export default function MemeGenerator() {
  const [text, setText] = useState("");
  const [image, setImage] = useState(null);
  const [memes, setMemes] = useState([]);

  const handleImageUpload = (event) => {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onloadend = () => {
        setImage(reader.result);
      };
      reader.readAsDataURL(file);
    }
  };

  const createMeme = () => {
    if (text && image) {
      setMemes([...memes, { text, image }]);
      setText("");
      setImage(null);
    }
  };

  return (
    <div className="p-4 max-w-xl mx-auto">
      <h1 className="text-2xl font-bold text-center mb-4">Gerador de Memes</h1>
      <Input
        type="text"
        placeholder="Digite o texto do meme"
        value={text}
        onChange={(e) => setText(e.target.value)}
        className="mb-2"
      />
      <Input type="file" accept="image/*" onChange={handleImageUpload} className="mb-2" />
      <Button onClick={createMeme} className="w-full">Criar Meme</Button>
      <div className="mt-4 grid gap-4">
        {memes.map((meme, index) => (
          <Card key={index} className="relative overflow-hidden">
            <CardContent>
              <img src={meme.image} alt="Meme" className="w-full rounded-md" />
              <motion.div
                className="absolute bottom-2 left-0 right-0 text-center text-white font-bold text-lg bg-black bg-opacity-50 p-2"
                initial={{ opacity: 0 }}
                animate={{ opacity: 1 }}
              >
                {meme.text}
              </motion.div>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  );
}
